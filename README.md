# omtober
Making Omarchy my daily driver

Source for [omtober.org](https://omtober.org): a journal of chantastic using
[Omarchy](https://omarchy.org) as a daily driver OS.

## Layout

```
public/            static site, served as-is (no build step)
  index.html       landing page + latest log entries
  log/index.html   journal index
  log/day-NN.html  one file per entry
  404.html         served for unknown paths
  styles.css       all styles
  favicon.svg
wrangler.jsonc     Cloudflare Worker (static assets only)
.github/workflows/deploy.yml   deploys on push to main
```

## Writing an entry

1. Copy `public/log/day-01.html` to `public/log/day-NN.html` and write.
2. Add a line to the list in `public/log/index.html` and `public/index.html`.
3. While an entry is a draft, keep the `DRAFT` banner, the `draft` tags, and
   `<meta name="robots" content="noindex">`. Remove all three to publish.

## Local preview

```sh
npx wrangler dev        # or: python3 -m http.server -d public
```

## Deploy

Pushes to `main` deploy via `cloudflare/wrangler-action`. The repo needs these
secrets: `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`.

Manual deploy: `npx wrangler deploy`.

### www → apex redirect

`_redirects` for Workers static assets doesn't support domain-level redirects,
so `www.omtober.org` → `omtober.org` is not in this repo. Add a Cloudflare
**Redirect Rule** on the zone instead (Rules → Redirect Rules → "Redirect from
WWW to root" template). Until then, both hostnames serve the same site;
canonical tags point at `https://omtober.org/`.
