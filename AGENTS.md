# AGENTS.md

abap2UI5 for decision makers: one static page that presents abap2UI5 to IT
management and product owners - what it can build, who runs it, where it is
the shorter path to a UI5 app on SAP and where it is not, what it costs -
published at https://abap2ui5.github.io/why/ by GitHub Pages. A
standalone site — its own repository, build and deployment — and nothing in
it depends on abap2UI5/docs or abap2UI5/playground at build time.

## Layout

| Path | Purpose |
| --- | --- |
| `src/pages/index.astro` | The home page: the text of every section, as data at the top and markup below |
| `src/pages/faq.astro` | The FAQ, grouped; the home page keeps a teaser at `#the-objections`, which is an address |
| `src/pages/community.astro` | Community & Support: the open-source idea, the support ladder, what it stands on, what to give back |
| `src/layouts/Page.astro` | The document: head, the Content-Security-Policy, the header with the section links, the foot |
| `src/components/` | `Stats` (the headline tiles under the hero), `Icon` (inline marks) |
| `src/styles/global.css` | The one stylesheet; light and dark follow the system |
| `public/` | Font (Inter, self-hosted), logo, icons, `apps/` (one screenshot per example app; the hero shows one of them) |
| `scripts/search-feed.mjs` | Runs after `astro build` and writes `dist/search.json`: one entry per section with an id and per FAQ question, read from the built pages. abap2UI5/docs fetches it at build time into the family's one search index (the home page and the playground read it); the shape `{ site, url, entries: [{ title, text, headings, terms, url }] }` is the same on every site of the family. A page that yields nothing fails the build |
| `test/site.test.mjs` | `node:test` over the built `dist/index.html` |
| `.github/workflows/pages.yml` | Build + test on every PR; deploy on main |

## Commands

```bash
npm ci
npm run dev      # dev server, http://localhost:4321/why/
npm run build    # dist/
npm test         # the tests, against dist/ - build first
npm run check    # build + test, what CI runs
```

## Rules

- **The eight section ids are addresses.** `/docs/evaluate` redirects here
  with its `#fragment`, and the framework's README links `#what-you-keep` and
  `#when-to-use-it-and-when-not`. Reword a heading freely; keep the `id`.
  `test/site.test.mjs` names all eight.
- **No script.** The policy is `script-src 'self'` and the page ships none:
  no inline script, no `on…=` handler. The FAQ and the folded references are
  `<details>`, the header is anchors. No inline `style=""` either
  (`style-src 'self'`) — a class in `global.css`.
- **Nothing from another origin.** No CDN, no web font service, no analytics;
  the font and the images are in `public/`.
- **Written for managers.** No technical counts (tests, specs, gates) and no
  "evaluate" or "problem" in the visible text - `test/site.test.mjs` checks the latter.
  Figures a reader can check (since when, contributors, the latest release) are
  fine; they carry their date and link to GitHub.
- **Short, honest, in this order.** A reader gives the page a few minutes:
  the visible text outside `<details>` stays under 4,000 words (the test holds
  it; the ceiling is generous for now - everything goes in first, a trimming
  pass follows later). The comparison names where abap2UI5 is *not* ahead (rows marked `even`,
  the "Where something else fits better" box) and the support it has: in the
  open first - anyone can open an issue or send a pull request on GitHub at any
  time - and more by mail to contact@abap2UI5.org on request; what is not on
  offer is an SLA in a contract (`SUPPORT.md` in the framework is the source).
  Never "no support": the open channel is the support, and the better one. Cost comes before AI; proof ("Who runs it") sits after the FAQ, just before Get started.
  No superlatives ("better than every", "made for"); a claim a reader cannot
  check is cut, not softened. Three things a reader asks first stay visible without scrolling into a
  section: the business logic stays in ABAP (hero, tile, a comparison row, the
  security heading), performance (a comparison row, a reason, a FAQ) and
  enterprise readiness (a section of its own at `#what-you-keep`, named in the navigation, with the review pack linked). Add-ons, Basis
  steps and other developer topics
  are links into the manual, not sections.
- **Every table cell carries its column.** `data-th` on each `<td>`: under
  700px a row becomes a card and the cell shows its column name. The test
  refuses a `<td>` without it.
- **Toolchain:** Node 22 (`.nvmrc`, `engines`), every dependency pinned
  exactly (astro 7.3.8), actions pinned to commits.
- **The one place for decision makers.** The manual (abap2UI5/docs) is for
  developers and links here; what used to be its Evaluate page, Who Uses and
  References lists lives on this page only, and its old addresses redirect
  here (the cost calculator's address lands on What it costs). Do not copy a developer topic in, link it.
- The site is written for people. Links into the manual are absolute
  (`https://abap2ui5.github.io/docs/...`).
