# agent-docs/

This folder is a condensed knowledge base for AI agents working on this repo. It is not meant for casual human browsing — the goal is to give an agent quick context on the current state of the codebase, past decisions, and ongoing work so it doesn't have to rediscover things from scratch each session.

## What belongs here

- **Trackers** for known issues (tech debt, bugs, accessibility, performance) with checkboxes so progress can be tracked across sessions
- **Decision logs** for significant architectural or content decisions (why something was done a certain way)
- **Change summaries** when a meaningful refactor or audit is completed

## What does NOT belong here

- Per-session notes or scratch work
- Anything that belongs in the root `CLAUDE.md` (project-wide conventions, environment setup)
- Content drafts or creative work

## Environment setup

If you're in a fresh environment (not the owner's desktop), run:

```
make setup    # runs: mise install && npm install
```

This installs Node, Python 3.12, and Hugo 0.146.1 via `mise.toml`, then installs the Node deps (markdownlint-cli, playwright-cli, tinacms).

## Validation workflow

Before and after any structural change, run:

```
make validate    # builds the site + checks all internal links
make lint        # markdownlint on content/
```

**Known baseline:** `make validate` passes with **zero broken internal links** (November 2026). Category links use Hugo term permalinks rather than manually deriving slugs. Treat new broken links as regressions. Stop the development server before validation so live-reload output cannot overwrite the production build during checks.

For risky changes (library upgrades, CSS refactors, template changes):

```
make snapshot              # saves public/ to public-snapshot/ before the change
# ... make changes ...
make diff-snapshot         # rebuilds and shows which HTML files changed
```

The diff output lists every HTML file that changed — use it to verify the scope of your changes matches expectations (e.g. a template change touching all blog posts is expected; a change touching unrelated pages is a red flag).

For visual verification (when the dev server is running via `make serve`):

`playwright-cli` is available for ad-hoc screenshots and DOM inspection. Use the make targets rather than raw commands:

```
make screenshot URL=http://localhost:1313 OUT=tmp/page.png    # screenshot any page
make pr-screenshots POST=http://localhost:1313/blog/my-post/  # homepage + post (for PRs)
```

For DOM inspection (snapshot, eval, network), use `npx playwright-cli <command>` directly — the Makefile targets only cover screenshot use cases. Run `npx playwright-cli --help` to see all available commands. No baseline to maintain — use your judgment.

**If opening a PR:** you are required to include screenshots of the homepage and a blog post page in the PR description. See the root `CLAUDE.md` for the full requirement.

## Files

- `tech-debt.md` — Audit of accessibility, performance, and tech debt issues found March 2026
- `nice-to-have.md` — Potential improvements that need owner decision before implementation (Hugo modernization, UX, SEO, security, DX)
