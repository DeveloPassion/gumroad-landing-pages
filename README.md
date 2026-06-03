# Gumroad Landing Pages — DeveloPassion / Knowii

Versioned archive and source of truth for every **custom Gumroad product landing page**.
One folder per product, keyed by the **Gumroad permalink** (the handle the CLI uses).

```
<permalink>/
  landing.html   # the published page (self-contained, sanitizer-safe)
  meta.json      # product id, name, store slug, landing_url, last_published, …
```

Example: [`cdwol/`](./cdwol) → Knowii – Journaling Deep Dive.

## Why this exists

A custom HTML page **replaces the entire Gumroad product page** (native price + buy button
included). Keeping each page here gives version history, easy rollback, and a single place to
edit/re-publish — independent of any local machine.

## Workflow

These pages are normally written by the `developassion-publish --target landing-page --publish`
skill, which renders/authors the page, previews it through Gumroad's sanitizer, publishes it, and
commits the result here. You can also manage a page by hand:

```sh
# Re-publish a stored page (preview first — inspect .sanitization_report AND .warning)
gumroad products page preview <permalink> <permalink>/landing.html --json --no-input --non-interactive
gumroad products page publish <permalink> <permalink>/landing.html --json --no-input --non-interactive

# Confirm it's live
gumroad products page url <permalink> --json --jq '.product.landing_url' --no-input --non-interactive

# Revert to Gumroad's default product page
gumroad products page clear <permalink> --yes --no-input --non-interactive
```

## Conventions

- **Permalink folders.** `cdwol/`, not `journaling-deep-dive/`. The human name lives in `meta.json`.
- **Self-contained `landing.html`.** Inline CSS/JS + Tailwind Play CDN only. Images must be the
  product's own Gumroad assets (`public-files.gumroad.com`), `data:` URIs, or CSS — external image
  hosts are blocked by the sandbox.
- **Always preview before publish** and check both `.sanitization_report` (clean) and `.warning`
  (no "no buy element"). Related repos: `DeveloPassion/gumroad-landing-template` (renderer/brand
  template), `DeveloPassion/store-website` (product data source of truth).
