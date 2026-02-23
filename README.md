# Laniakea Beach House Static Site

Static rebuild of `https://www.laniakeabeachhouse.com/` for Cloudflare Pages deployment.

## What is included

- Three routes:
  - `/`
  - `/rooms-beds/`
  - `/videos/`
- Official Webflow export integrated into:
  - `static/index.html`
  - `static/rooms-beds.html`
  - `static/videos.html`
  - `static/css/`
  - `static/js/`
  - `static/images/`
- Localized asset dependencies (no runtime dependency on Webflow/CDN asset hosts).
- Existing video embeds and third-party links are preserved.

## Tech stack

Recommended for this project:

- Hosting/CDN: Cloudflare Pages
- Site runtime: Static HTML/CSS/JS (no framework runtime)
- Optional upgrade if you want easier content editing later: Astro with content collections

This keeps cost and complexity low while preserving current design/behavior.

## Local preview

```bash
python3 -m http.server 4173 --directory static
# open http://localhost:4173
```

Note: Python's static server does not apply Cloudflare `_redirects`.
For local testing with the same routes as Pages, open:

- `http://localhost:4173/`
- `http://localhost:4173/rooms-beds.html`
- `http://localhost:4173/videos.html`

## Deploy to Cloudflare Pages

This project is static-only. Use Cloudflare Pages with build output directory `static`.

### Option A: Git-based deploy (recommended)

1. In Cloudflare Dashboard:
   - Go to `Workers & Pages` -> `Create` -> `Pages` -> `Connect to Git`.
   - Select repo: `justin808/laniakeabeachhouse`.

2. Build settings:
   - Production branch: `main`
   - Build command: `exit 0`
   - Build output directory: `static`

3. Save and deploy.

Cloudflare will now auto-deploy on pushes to `main`.

### Option B: Manual deploy from CLI

1. Authenticate:

```bash
npx wrangler whoami
# if not logged in:
npx wrangler login
```

2. First deploy (project will be created if needed):

```bash
npx wrangler pages deploy static --project-name laniakea-beachhouse
```

You can also use:

```bash
npm run deploy
```

### Staging first, then production cutover

Recommended rollout:

1. Add `staging.laniakeabeachhouse.com` as a custom domain in the Pages project.
2. Verify staging.
3. Add production domains:
   - `laniakeabeachhouse.com`
   - `www.laniakeabeachhouse.com`
4. Configure one canonical redirect (`www` -> apex, or apex -> `www`).

Notes:
- Apex/root domain requires your DNS zone on Cloudflare nameservers.
- For local preview, python server does not apply `_redirects`.
- On Cloudflare Pages, `_redirects` and `_headers` in `static/` are applied.

### Post-deploy verification checklist

Check these routes:
- `/`
- `/rooms-beds`
- `/videos`

Check these assets:
- `/css/laniakea-beach-house.webflow.css`
- `/js/webflow.js`
- `/images/Outside-Jockos-big-swell.jpeg`

Expected behavior:
- `200` on pages and assets
- no `404` for referenced images/scripts/styles
- custom domains show valid HTTPS certificate

## Notes

- Webflow export assets are now served locally from this repo.
- Remaining external runtime calls are third-party embeds/integrations (for example YouTube and Google APIs).
