# Seeds base

The foundation of a Seeds site, as a recipe. It replaces the Seeds install profile.

It sets up what a fresh Seeds 13 install has. This was checked config item by config item:

- All Seeds modules (77): Seeds Media, Editor, Pollination, Development, Security and Toolbar,
  Display Suite, Field Group, Diff, Masquerade, Password Policy, CAPTCHA and the rest
- **Seeds UI** as the front-end theme and **Default Admin** as the admin theme
- Seeds' media types, image styles, text formats and editors
- Seeds Toolbar sidebar, navigation and permissions
- Security settings (SecKit, Restrict IP, Session limit, two-factor email)
- Front page `/user`; the default Frontpage and Taxonomy term views are disabled
- The Seeds UI `blazy` setting, so pages do not show PHP warnings

## How it was built

It is generated from the active configuration of a freshly installed Seeds 13.2.3 site
(`drush config:export`). Config entities are shipped in `config/`. Settings are applied with
config actions, because modules create their settings when they are installed.

Three settings have no config schema (`media_embeddable.settings`, `seeds_media.settings`,
`seeds_pollination.settings`), so a recipe cannot set them. A fresh Seeds site uses their
defaults anyway. Adding a schema to those modules would remove this limit.

## Use

Usually through **Seeds template**. On its own:

```bash
drush site:install ../recipes/seeds_base
```

## Known issues in Seeds modules

- `seeds_toolbar` 3.0.1 and 3.0.2 crash on install: they use the Update module's
  `UpdateManagerInterface` constants before Update is enabled. Patch until fixed upstream.
- `restrict_ip` 5.0.1 breaks every page once Drupal Canvas is enabled: it type-hints the
  concrete `Messenger` class. Patch until fixed upstream.
