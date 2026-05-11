# NextJS for Drupal site template

A Drupal recipe that pairs with a Next.js front end. Build pages with Drupal Canvas Code Components, render them with Next.js.

This recipe is a thin wrapper around the [`drupal/ui` recipe](https://www.drupal.org/project/ui). The `ui` recipe ships the 43 Canvas Code Components, the demo home page, and the editor/admin scaffolding. This recipe layers on the decoupled bridge — `next`, `jsonapi`, `decoupled_router`, `subrequests`, and the [`kanopi/next_canvas`](https://github.com/kanopi/next_canvas) module that exposes the REST endpoints the front end consumes.

> **Status:** Currently hosted on Kanopi GitHub. Once the `nextjs` namespace on drupal.org is approved, this recipe will move to `drupal/nextjs`. The internal API (recipe.yml, configs) will not change.

## What this installs

### Recipes applied

- `ui` — Canvas Code Components library + editor scaffolding + home page.
- `drupal_cms_authentication`
- `drupal_cms_seo_basic`
- `easy_email_express`

### Modules enabled

| Module | Purpose |
|---|---|
| `next` | Site connection, preview, and revalidation. |
| `jsonapi` | Default Drupal JSON:API. |
| `decoupled_router` | Path resolution for the front end. |
| `subrequests` | Batch API requests. |
| `next_canvas` | REST endpoints for site branding, page regions, page meta, media resolver. |

### Config

- A `next_site` config named `canvas_starter` pointing at `http://localhost:3000` with the standard draft/revalidate URLs.
- An entity-type-to-Next-site mapping that routes `canvas_page` entities to the `canvas_starter` site.

## Installing

Until this is on packagist, your consuming site needs to declare the Kanopi GitHub repos:

```json
"repositories": [
  { "type": "vcs", "url": "https://github.com/kanopi/nextjs" },
  { "type": "vcs", "url": "https://github.com/kanopi/next_canvas" }
]
```

Then:

```bash
ddev composer require kanopi/nextjs
ddev drush recipe recipes/contrib/nextjs
```

If you're starting from scratch with `drupal/cms`, select **NextJS for Drupal** from the installer's template picker.

### Post-install setup

Generate OAuth keys (required for the Canvas CLI and draft preview):

```bash
ddev exec mkdir -p /var/www/html/keys
ddev drush simple-oauth:generate-keys /var/www/html/keys
ddev drush config:set simple_oauth.settings public_key /var/www/html/keys/public.key
ddev drush config:set simple_oauth.settings private_key /var/www/html/keys/private.key
```

## Wiring up the front end

Pair this with the [`kanopi/drupal-canvas-nextjs`](https://github.com/kanopi/drupal-canvas-nextjs) Next.js starter. Point its `NEXT_PUBLIC_DRUPAL_BASE_URL` at this Drupal site, run `npm run dev`, and you're rendering Canvas pages from Drupal in Next.js.

Or use [`kanopi/next-canvas-dev`](https://github.com/kanopi/next-canvas-dev) — the sibling-clone dev environment that wires up Drupal, this recipe, the front end, and the bridge module in one bootstrap step.

## Requirements

- Drupal CMS 2.1 or later (Drupal ^11)
- PHP 8.3+
- Composer
- `drupal/ui` ^1
- `kanopi/next_canvas` ^1 (or `dev-main`)

## License

GPL-2.0-or-later.
