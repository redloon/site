# CLAUDE.md

This is a personal website built with the static site generator Hugo.

Use `agent-docs/` as a reference, since it's meant for documenting specific knowledge useful for agents working in this repository.

## History of the repo

 It was originally started from a theme saved in `themes/hugo-goa/`, but over time this has morphed and been modified heavily, so I can't remember ever bothering to pull in updates from the original - and at this point, they might break more than they help.

At one point I added Tina, which lets you run a local CMS for easier editing. I also often just manually add new content in VSCode.

Tina is rarely used day-to-day anymore — editing is almost always done directly in VSCode. The current Netlify build runs `hugo --gc --minify`; Tina remains available for local editing.

## Environment

- Hugo `v0.146.1` and Node are managed via `mise` (`mise.toml`). Run `make setup` in a fresh environment to install all tools.
- The `hugo/` directory in the repo root is gitignored — it's a leftover from when the binary was installed manually there. Don't rely on it; use `mise` instead.
- The `mise` tool is available for you to use.
- It deploys to Netlify whenever a push is made to the default branch (`master`). Pull request deploy previews are working (confirmed November 2026).
- `artifacts/` is for things I want to keep in the repo, but which don't get served on the site, e.g. original copies of Excalidraw files that were used as the source to generate images.
- I have added [playwright-cli](https://github.com/microsoft/playwright-cli) to the Node dependencies of this repo. Use `make screenshot` and `make pr-screenshots` targets rather than composing raw `npx playwright-cli` commands — see the Testing & Validation section below.

## Repo info

- I like to occassionally add public HTML tools & pages to my site. Those end up in `/static/bin` since it's an easy way to host them.
- I host a sub-website with `content/indie-lurker/`, since I didn't want to maintain a separate domain any longer. It's treated as its own unique site, not meant to match the rest of the website.
- New blog posts are created manually in VSCode under `content/blog/`. Frontmatter can be either TOML (`+++` delimiters) or YAML (`---`) — no strong preference, but existing posts mostly use TOML. Standard fields: `title`, `date`, `description`, `categories`, `tags`, `images`, `draft`, `featured`, `toc`.
- Images for posts go in `/static/uploads/` (legacy Tina location) or `/static/img/`, and are referenced in content as `/uploads/filename.ext` or `/img/filename.ext`.
- Custom shortcodes live in `layouts/shortcodes/` — notable ones: `tenor-gif`, `giphy`, `video`, `twocol`, `bootstrap-panel`, `notion-callout`, `new_tab_link`.

## Developer info

- The site is using very old versions of Boostrap and other libraries, since it grew off of theme that I never updated. I have some slight concern about that, but also I don't want to update and ruin all the styling and things I already have in place, so it hasn't been a pressing issue for me. I might just archive a copy of those dependencies so that I have it in the repo if CDNs ever stop serving them.

## Testing & Validation

No unit tests. Use these make targets:

- `make validate` — builds the site and checks all internal links for breakage (run before/after structural changes)
- `make lint` — runs markdownlint on content
- `make snapshot` then `make diff-snapshot` — for risky changes (CSS, library upgrades): save a build before, diff after to see exactly which pages changed
- `make clean` — wipe generated output for a fresh build
- `make serve` — local dev server at localhost:1313
- `make screenshot URL=http://localhost:1313 [OUT=tmp/screenshot.png]` — screenshot a page (requires server running)
- `make pr-screenshots [POST=http://localhost:1313/blog/your-post/]` — take homepage + blog post screenshots for PR descriptions

**Principle: prefer `make` targets over composing raw commands.** If a workflow isn't captured in the Makefile, add a target rather than documenting a multi-step raw command sequence. This keeps the process reproducible and avoids shell environment issues (PATH, mise activation, etc.).

See `agent-docs/CLAUDE.md` for the full playwright workflow and the baseline broken-link count.

## Pull Requests

If you are opening a pull request, you **must** include screenshots of the following pages in the PR description. Start the server with `make serve`, then run:

```
make pr-screenshots POST=http://localhost:1313/blog/<recent-post-slug>/
```

This saves `tmp/pr-homepage.png` (homepage) and `tmp/pr-blog.png` (blog post). Embed both in the PR description.

This lets the site owner review visual state without having to run the server themselves.