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

1. Authenticate:

```bash
npx wrangler whoami
# if not logged in
npx wrangler login
```

2. Create a Pages project (first time only):

- Cloudflare Dashboard -> Workers & Pages -> Create application -> Pages -> Upload assets
- Project name suggestion: `laniakea-beachhouse`

3. Deploy from CLI:

```bash
npx wrangler pages deploy static --project-name laniakea-beachhouse
```

4. Attach your custom domain in the Pages project settings.

## Notes

- Webflow export assets are now served locally from this repo.
- Remaining external runtime calls are third-party embeds/integrations (for example YouTube and Google APIs).
