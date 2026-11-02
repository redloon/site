# Tech Debt Tracker

Audited: 2026-03-04. Updated: 2026-11-02. The straightforward accessibility, navigation, and performance fixes are complete. Remaining library upgrades and video caption authoring are explicitly open below.

---

## Accessibility

### Reading images and navigation (November 2026)
- [x] Added descriptions to all ten Markdown images with empty alt text; improved three vague descriptions and the homepage portrait alternative.
- [x] Added visible figure captions to the salary charts, Venmo summary, and serverless fan-out diagram. Keep detailed explanations and pseudocode in surrounding text.
- [x] Fixed all 65 reported internal-link failures: use Hugo category term permalinks, correct article/static-tool URLs and anchors, and disable nonexistent mockup destinations. `make validate` now passes.
- [x] Audited generated HTML: all informative article images have nonempty alternatives. Header social SVGs intentionally use empty alternatives inside links with accessible names. Third-party injected images remain outside this static check.

### Iframes missing `title` attribute
- [x] [MED] `themes/hugo-goa/layouts/partials/email-form.html:18` — Beehiiv iframe was already commented out; now using Kit.com JS embed (no iframe). Non-issue.
- [x] [MED] `layouts/shortcodes/giphy.html` — fixed: added `title="Giphy GIF embed"`
- [x] [MED] `layouts/shortcodes/tenor-gif.html` — use a directly titled, lazy-loaded Tenor iframe with a descriptive fallback link. The existing embed supplies a description as its third positional argument; no MutationObserver is needed.
- [x] [LOW] `static/bin/bus.html:59` — fixed: added `title="Chicago Transit bus tracker"`

### Missing `lang` attribute on `<html>` tag
- [x] [MED] `static/bin/index.html` — fixed
- [x] [MED] `static/bin/bus.html` — fixed
- [x] [MED] `static/bin/points-depreciation.html` — fixed

### Broken or incorrect ARIA
- [x] [HIGH] `themes/hugo-goa/layouts/partials/content.html:74` — fixed: `ariaLabel=` → `aria-label=`; affects all blog post heading anchors
- [x] [LOW] `themes/hugo-goa/layouts/partials/header.html:81` — logo `<img>` has both `alt` and `aria-label`; remove `aria-label`, `alt` is sufficient

### Images with empty or missing alt text
- [x] [HIGH] `themes/hugo-goa/layouts/partials/content.html` — optional header art accepts `headerimagealt`; empty alt is deliberate for decorative art. `showheaderimage = false` is now honored. No published pages currently enable this optional header art.

### Missing form labels
- [x] [MED] `layouts/partials/pricing-calculator.html` — associated the Total Users label with `numUsersInput`.

### Video accessibility
- [ ] [LOW] `layouts/shortcodes/video.html` — the recorder performance still needs an authored, reviewed caption/audio-description track. No placeholder or invented transcript has been added.

### Broken icon rendering
- [x] [MED] `themes/hugo-goa/layouts/partials/header.html` — the configured Twitter/GitHub header links now use the existing local SVG assets rather than missing FontAwesome icons.

---

## Performance

### Image optimization
- [x] Converted the five currently displayed large assets below to lossless WebP, preserving decoded pixels and original URLs/files for existing links. Combined size falls from 2,558,950 to 1,302,874 bytes (49% smaller):
  - `error-blame-game-tennis.png` → `.webp`
  - `chestertons-fence-illustrated.png` → `.webp`
  - `recurring-venmo-payments-smaller.png` → `.webp`
  - `hugo-site-random-button-small.jpg` → `.webp`
  - `puma-puff-cropped.png` → `.webp`
- [x] Losslessly optimized `kq-fun-default-site-img.png` from 151,990 to 121,784 bytes. Keep PNG for social-card compatibility. Original JPEG/PNG assets used in social metadata also remain available.
- [x] Reviewed the other old candidates: `pihole-dashboard.png` has no current site references; `hai-nguyen-lszfnpvzjtw-unsplash.jpg` is only social metadata. Neither is part of the page-reading download path.

### Lazy loading
- [x] [MED] `themes/hugo-goa/layouts/_default/_markup/render-image.html` — rendered images have no `loading="lazy"` attribute; add it to defer offscreen images

### Missing SRI hashes (security + cache integrity)
- [x] [MED] `themes/hugo-goa/layouts/partials/footer.html` — pinned jQuery bytes with SHA-384 integrity and anonymous CORS; retained the existing version.
- [x] [MED] `themes/hugo-goa/layouts/partials/footer.html` — pinned Highlight.js bytes with SHA-384 integrity and anonymous CORS; retained the existing version.

### Google Fonts API
- [x] [LOW] `themes/hugo-goa/layouts/partials/header.html:39` — Fonts URL has a duplicate `rel` attribute (both `rel="stylesheet"` and `rel="preconnect"` on the same tag) and is missing `display=swap`. Fix: split into a `<link rel="preconnect">` and a separate `<link rel="stylesheet" href="...&display=swap">`.

---

## Tech Debt

### Severely outdated CDN dependencies
These are load-bearing and will require careful testing if upgraded. The site's styling is tightly coupled to Bootstrap 3.

- [ ] [HIGH] **Bootstrap 3.3.7** (2014) — used in `themes/hugo-goa/layouts/partials/header.html` + `footer.html`. Current: Bootstrap 5.x. Upgrading is a breaking change to grid classes and component markup; track separately if attempted.
- [ ] [HIGH] **jQuery 1.12.4** (2016) — `themes/hugo-goa/layouts/partials/footer.html:31`. Current: 3.7+. Check if any site JS actually depends on jQuery before deciding to upgrade vs. remove.
- [ ] [MED] **Highlight.js 9.12.0** (2017) — `themes/hugo-goa/layouts/partials/footer.html:20`. Current: 11.x. Upgrade is likely low-risk.

### Dead code: IE9 compatibility shims
- [x] [LOW] `themes/hugo-goa/layouts/partials/header.html:48-52` — HTML5shiv and Respond.js loaded via conditional comments for IE8/9. IE is dead (EOL 2022). Safe to delete the entire block.
- [x] [LOW] `themes/hugo-goa/static/css/main.css` — contains `@-ms-viewport` and `@-o-viewport` rules for Windows Phone / Opera Mobile. Dead code, safe to remove.
- [x] [LOW] `themes/hugo-goa/static/js/main.js:3` — IE10 Mobile viewport fix. Safe to remove.

### Dead CMS config
- [x] [LOW] `.forestry/` — Forestry.io was acquired/deprecated. This directory is entirely dead config. Safe to delete.

### Tina CMS config issues
- [x] [LOW] `tina/config.ts:5` — hardcoded `"main"` as the branch name, but this repo uses `master`. Should be `process.env.HEAD || "master"`.
- [x] [LOW] `tina/config.ts:6` — fallback token is `"blech"` (a placeholder). Fine locally, but worth removing the fallback so it fails loudly in misconfigured environments.

### Stale content
- [x] [LOW] The old stale-draft entry is obsolete: the article is published at `content/til/my-backup-plan-if-dex-crm-goes-kaput.md` with `draft = false`.

### Node version
- [x] [LOW] `.nvmrc` specifies `18.12.0` (Oct 2022). `mise.toml` overrides this locally with `node = "latest"`. Update `.nvmrc` to a current LTS (20.x or 22.x) so it doesn't mislead.

### Netlify deploy previews
- [x] [LOW] Deploy previews are working. PR #3 is available at https://deploy-preview-3--kevinquinn.netlify.app/ (confirmed November 2026). No Netlify configuration changes needed.

### GitHub identity and reading controls
- [x] Updated GitHub account/repository/Pages URLs and package metadata from the former username to `redloon`. Stack Overflow and Ballerina identifiers belong to separate services and are unchanged.
- [x] Removed the Skip to article link and its CSS at the owner's request. Retained the main landmark and ordinary keyboard focus styling.
