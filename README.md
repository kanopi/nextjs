# NextJS for Drupal recipe

A Drupal recipe that adds a decoupled Next.js bridge on top of the [`drupal/ui`](https://www.drupal.org/project/ui) recipe (which brings shadcn/ui components into Drupal Canvas). Pair it with the [`kanopi/drupal-canvas-nextjs`](https://github.com/kanopi/drupal-canvas-nextjs) front end and you have a working decoupled Drupal + Next.js stack.

> **Status:** Currently hosted on Kanopi GitHub. Once the `nextjs` namespace on drupal.org is approved, this recipe will move to `drupal/nextjs`. The internal API (recipe.yml, configs) will not change.

## What this installs

- **Applies the `ui` recipe** — components, global CSS, page region takeover for Stark's header/footer, OAuth scope, demo home page.
- **Enables the decoupled bridge modules**:
  - `next` — site connection, draft preview, revalidation.
  - `jsonapi` — Drupal core's REST API.
  - `decoupled_router` — front-end path resolution.
  - `subrequests` — batched API requests.
  - `next_canvas` — REST endpoints for site branding, page regions, page meta, and media resolver (the front end calls these).
- **Ships a Next.js site connection** at `http://localhost:3000` (`next.next_site.canvas_starter`).
- **Routes `canvas_page` entities to that site** with draft preview + path revalidation (`next.next_entity_type_config.canvas_page.canvas_page`).

## Installing

Until this is on packagist, your consuming site needs the Kanopi GitHub repos declared as VCS sources:

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

### Post-install: generate OAuth keys

Needed for the Canvas CLI and Next.js draft preview:

```bash
ddev exec mkdir -p /var/www/html/keys
ddev drush simple-oauth:generate-keys /var/www/html/keys
ddev drush config:set simple_oauth.settings public_key /var/www/html/keys/public.key
ddev drush config:set simple_oauth.settings private_key /var/www/html/keys/private.key
```

## Run the front end

Pair with [`kanopi/drupal-canvas-nextjs`](https://github.com/kanopi/drupal-canvas-nextjs):

```bash
cd path/to/drupal-canvas-nextjs
cp .env.example .env.local   # set NEXT_PUBLIC_DRUPAL_BASE_URL to your Drupal site
npm install
npm run dev
```

Or use [`kanopi/next-canvas-dev`](https://github.com/kanopi/next-canvas-dev) — the sibling-clone dev environment that wires up Drupal + this recipe + the front end + the bridge module in one bootstrap step.

## Requirements

- Drupal ^11
- `drupal/ui` ^1
- `kanopi/next_canvas` ^1 (or `dev-main`)
- `drupal/next` ^2
- `drupal/decoupled_router` ^2
- `drupal/subrequests` ^3

## License

GPL-2.0-or-later.
