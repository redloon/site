# Tech Debt Tracker

Audited: 2026-03-04. Issues found across accessibility, performance, and general tech debt. Check items off as resolved.

---

## Accessibility

### Reading images and navigation (November 2026)
- [x] Added descriptions to all ten Markdown images with empty alt text; improved three vague descriptions and the homepage portrait alternative.
- [x] Added visible figure captions to the salary charts, Venmo summary, and serverless fan-out diagram. Keep detailed explanations and pseudocode in surrounding text.
- [x] Fixed all 65 reported internal-link failures: use Hugo category term permalinks, correct article/static-tool URLs and anchors, and disable nonexistent mockup destinations. `make validate` now passes.
- [x] Checked generated HTML: all 377 rendered image occurrences have nonempty alternatives. This does not cover images injected later by third-party scripts.

### Iframes missing `title` attribute
- [x] [MED] `themes/hugo-goa/layouts/partials/email-form.html:18` — Beehiiv iframe was already commented out; now using Kit.com JS embed (no iframe). Non-issue.
- [x] [MED] `layouts/shortcodes/giphy.html` — fixed: added `title="Giphy GIF embed"`
- [ ] [MED] `layouts/shortcodes/tenor-gif.html` — Tenor's iframe is injected dynamically by `embed.js` after page load; no iframe exists in the template to add a title to. Would need a MutationObserver hack or a different embed approach to fix properly.
- [x] [LOW] `static/bin/bus.html:59` — fixed: added `title="Chicago Transit bus tracker"`

### Missing `lang` attribute on `<html>` tag
- [x] [MED] `static/bin/index.html` — fixed
- [x] [MED] `static/bin/bus.html` — fixed
- [x] [MED] `static/bin/points-depreciation.html` — fixed

### Broken or incorrect ARIA
- [x] [HIGH] `themes/hugo-goa/layouts/partials/content.html:74` — fixed: `ariaLabel=` → `aria-label=`; affects all blog post heading anchors
- [x] [LOW] `themes/hugo-goa/layouts/partials/header.html:81` — logo `<img>` has both `alt` and `aria-label`; remove `aria-label`, `alt` is sufficient

### Images with empty or missing alt text
- [ ] [HIGH] `themes/hugo-goa/layouts/partials/content.html:66` — post images rendered with `alt=""` (empty string); should be descriptive or at minimum omitted for decorative images

### Missing form labels
- [ ] [MED] `layouts/partials/pricing-calculator.html` — number input (`id="numUsersInput"`) has no associated `<label>`

### Video accessibility
- [ ] [LOW] `layouts/shortcodes/video.html` — `<video>` element has no `<track kind="captions">` fallback

### Broken icon rendering
- [ ] [MED] `themes/hugo-goa/layouts/partials/header.html:41` — FontAwesome 4.6.3 CSS is commented out, but `fa fa-twitter`, `fa fa-github`, etc. icons are still referenced in templates; social icons likely render as blank boxes. Either re-enable the CSS or replace with inline SVGs.

---

## Performance

### Image optimization
- [ ] [HIGH] Multiple large images in `static/uploads/` not converted to WebP. Worst offenders:
  - `error-blame-game-tennis.png` — 760K
  - `chestertons-fence-illustrated.png` — 517K
  - `recurring-venmo-payments-smaller.png` — 525K
  - `hai-nguyen-lszfnpvzjtw-unsplash.jpg` — 637K
  - `hugo-site-random-button-small.jpg` — 489K
  - `pihole-dashboard.png` — 370K
- [ ] [MED] Images in `static/img/` also not WebP:
  - `puma-puff-cropped.png` — 211K
  - `kq-fun-default-site-img.png` — 149K

### Lazy loading
- [x] [MED] `themes/hugo-goa/layouts/_default/_markup/render-image.html` — rendered images have no `loading="lazy"` attribute; add it to defer offscreen images

### Missing SRI hashes (security + cache integrity)
- [ ] [MED] `themes/hugo-goa/layouts/partials/footer.html:31` — jQuery CDN link has no `integrity` / `crossorigin` attributes
- [ ] [MED] `themes/hugo-goa/layouts/partials/footer.html:20` — Highlight.js CDN link has no `integrity` / `crossorigin` attributes

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
- [ ] [LOW] `content/blog/my-backup-plan-if-dex-crm-goes-kaput.md` — has been `draft = true` since 2023. Review: finish, archive, or delete.

### Node version
- [x] [LOW] `.nvmrc` specifies `18.12.0` (Oct 2022). `mise.toml` overrides this locally with `node = "latest"`. Update `.nvmrc` to a current LTS (20.x or 22.x) so it doesn't mislead.

### Netlify deploy previews
- [x] [LOW] `netlify.toml` — deploy preview context has a `# TODO: doesn't work` comment. Either fix or remove the `[context.deploy-preview]` block to avoid confusion. Removed the block entirely.
