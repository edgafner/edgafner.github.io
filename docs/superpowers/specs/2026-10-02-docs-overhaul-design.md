# DORKAG docs overhaul: design

Date: 2026-10-02.
Branch: `feature/docs-overhaul-2026-10` (docs), `feature/doc-screenshots-2026-10` (each plugin repo, local only).
Status: approved by a specialist review standing in for the owner, who was away and asked for decisions to be delegated. The owner reviews it asynchronously.

## 1. Intent

**Outcome.**
The site at https://edgafner.github.io gives accurate, complete, task-oriented documentation for AZD, GBrowser, JirAI, Codecov and QueryFlag, and the home page presents the plugins well.
Screenshots are fresh and consistent. They are captured by automated UI tests and follow JetBrains guidelines.

**The owner said:**
- Read each plugin's features and recent changelogs.
- Retake screenshots with UI tests, following the JetBrains guidelines.
- Ask the AZD and JirAI sessions and specialist subagents when a decision is unclear.
- Use the remaining token budget to improve the docs.

**Assumptions:**
- Merging to `main` deploys publicly, so the owner merges. We deliver PRs.
- Plugin repositories get only local branches with the screenshot tests. They are not pushed.
- Live-data screenshots for AZD, Codecov and JirAI need tokens. The owner was asked for them; until then we capture only credential-free screens.

**Success criteria:**
1. Every user-visible feature of the latest released version of each plugin is documented, or deliberately left out for a stated reason.
2. Every factual statement agrees with the code at origin/main and with the released changelog.
3. No image shows wrong content.
   Every new screenshot follows the spec in section 3 and has a `_dark` twin.
4. The Writerside build (builder 2026.09.0357) reports 0 errors and the checker passes.
5. The work is delivered as a PR with a per-plugin summary, plus a list of open questions for the owner.

## 2. Findings that drive the design

The audits from 2026-10-02 cover each plugin's code against its docs, the guidelines, a site-wide audit, and the UI-test infrastructure.

| Plugin | Doc state | Errors found | Screenshot state |
|---|---|---|---|
| AZD | 2024–25 era. Every Settings path is wrong; the real one is `Settings \| Version Control \| AZD \| …`. Boards, Azure CLI sign-in, pipeline metrics and agent pools, YAML language server, licensing, AI Auto Review, MCP and quick filters are missing. | about 100 | All 14 `*_2026.png` files are byte copies of unrelated images. 2 of them are published under wrong captions. Most of the rest is stale. |
| JirAI | Good as of 2026.3.11. 2026.3.12–3.16 are missing: Board, Breakdown and hierarchy, agent tools, house standard, account management. | 31 | Whole-IDE 1920×950 captures with probe data and the author's name. |
| Codecov | Thin. Code Vision, the gutter popup, AI prompts, metrics, folder coverage, upload and licensing are missing. | 33 | Show a private repo and macOS chrome. |
| GBrowser | About 2 years stale. Device emulation, page theme, Open in GBrowser, compatibility mode and request headers are missing. | 24 | 2023–24 images; a 6.2 MB GIF. |
| QueryFlag | Main path only. The Settings path and the SQL examples are wrong. | 23 | Old UI, a private hostname, a 51 px sliver. |

**Site-wide findings:**
- No `web-summary`, `link-summary` or `card-summary` on any of the 43 topics, so `og:description` is empty everywhere.
- No `sitemap.xml` and no `llms.txt`. The pending `<llms-txt>true</llms-txt>` inside `<variables>` has no effect; it must be a direct child.
- Wrong brand casing: "Azd", "Queryflag".
- Version labels are stale.
- `keymap.xml` is referenced but missing.
- `images/azd/` and `images/codecov/` hold 60 dead duplicates. Writerside always picks the root file.
- JirAI links point to a private GitHub repo, so public users get a 404.
- The home page is a `<list>`, not a starting page, and its icons mix PNG and SVG.

**Constraints:**
- The only display is a 1x 1920×1080 monitor.
- `Driver.takeScreenshot` is a full-screen Robot capture at logical resolution.
- IDE Starter kills every process with an `ide-tests` path segment when a test class starts or ends.
- The screen locks immediately once the display sleeps, which happens after 120 minutes idle. Robot input during test runs counts as activity.

## 3. Decisions

### D1. Screenshot spec

| Item | Decision |
|---|---|
| IDE | IntelliJ IDEA Ultimate EAP **IU-263.6259.32** for every plugin (the older pins expire). New UI, fresh config, only the plugin under test installed. |
| Themes | Two runs of the same test. **Islands Light** produces `name.png`; **Islands Dark** produces `name_dark.png`, which Writerside switches to automatically. |
| Window | Fixed **1280×800** logical (16:10, above the Marketplace minimum of 1200×760), centred. Never maximised. |
| Resolution | **1x**. The 144-DPI (2x) rule cannot be met on this display; see "Deviation" below. |
| Crop | A tight crop of the component the step discusses (dialog, tool window, popup, editor with gutter) plus 8–16 px of context. At most 1–2 whole-window overview shots per plugin. Prefer widths of 706 px or less. |
| Clutter | No balloons, memory indicator, tips, onboarding, cursor, desktop, menu bar or Dock. Window corners are masked to the rounded radius. |
| Data | Neutral sample data: a folder called `demo-project` with realistic content. No real names, e-mails, avatars, tokens or hostnames. |
| Naming | `<plugin>_<area>_<subject>[_<state>].png`, lowercase snake_case, globally unique across `Dorkag/images/**`. No dates or versions. |
| Storage | `Dorkag/images/<plugin>/` (new folders `gbrowser/`, `queryflag/`, `jirai/`; `azd/` and `codecov/` after their dead duplicates are removed). |
| Markup | `<img src="name.png" alt="One sentence: UI plus state" width="W" border-effect="rounded"/>`. `W` is the pixel width capped at 706. Add `thumbnail="true"` when the image is wider than 706 px. |
| Weight | `oxipng -o 4 --strip safe`. |

**Deviation.**
JetBrains docs use 2x PNGs, but this machine cannot produce them.
Robot capture is 1x, and a 2x window would not fit the screen.
A possible follow-up is a small helper plugin that paints Swing components into a 2x offscreen image (`printAll` with `scale(2,2)`). It would not capture JCEF pages.
We ship 1x now. The test harness can be re-run later at 2x on a Retina display or with that helper.

### D2. Capture harness

- Each plugin repo gets a `src/uiTest/.../docshots/DocScreenshots.kt` helper and one `<Plugin>DocScreenshotsUI` test, on the local branch `feature/doc-screenshots-2026-10`.
- Runs go through `docshots.sh`. It holds `~/.cache/dorkag-uitest/screen.lock` and waits until no foreign `ide-tests` process exists. Peer sessions were asked to use the same lock; all four confirmed they have no UI-test runs planned.
- Credential-free shots are captured now. These are settings pages, sign-in dialogs, AI provider dialogs in AZD mock mode, not-connected tool windows, and everything in GBrowser and QueryFlag.
- Live-data shots run in a second pass once the owner provides `AZD_TOKEN`, `CODECOV_API_TOKEN`, and the Jira token.
- Until then, a referenced image that is not available is handled by its audit verdict:
  - An old image that is still correct (verdict `ok`, or `stale-UI` with the right concept) stays.
  - Images with the verdict `wrong-content` are removed.

### D3. Site-wide Writerside upgrade (owned by the site writer)

- Builder **2026.09.0357** in CI and in the docs. This carries over the owner's pending upgrade: the docker bump, `smart-ignore-vars`, the `.gitignore` additions and the sortable tables.
- `<llms-txt/>` and `<sitemap priority="0.35" change-frequency="monthly"/>` as direct children of `<buildprofiles>`.
- In `<variables>`: `<product-web-url>`, and `<og-image>` as an absolute 1200×630 PNG URL.
- A `<web-summary>`, `<link-summary>` and `<card-summary>` in every topic. These are written by the topic owners.
- In `labels.list`: AZD and QueryFlag casing, and the current versions (AZD 2026.3.6, Codecov 2026.3.2, JirAI 2026.3.16; GBrowser and QueryFlag unchanged).
- In `c.list`: add `<category id="related" name="Related topics" order="0"/>`.
- Remove the dangling `<shortcuts>` block that points to the missing `keymap.xml`. Shortcuts stay as literal `<shortcut>` text.
- Delete the dead duplicate folders (`images/azd/*` and `images/codecov/*`, which are byte or recompressed copies of root files) and unreferenced images that no topic uses after the rewrite. Git history keeps them.
- Not in scope: the legal texts (`privacy-policy.topic`, `terms-of-service.topic`). Their issues go to the owner as questions.

### D4. Home page

`Dorkag.topic` becomes a `<section-starting-page>`:
- The description says what DORKAG is.
- Spotlight: AZD and JirAI, the two flagship paid plugins.
- Primary: all five plugins, each with an SVG icon and a one-line summary.
- Secondary: support and legal.
- Misc: a short "About" line. "Built and maintained by Jonathan Gafner" is kept; "in his free time" is dropped because it conflicts with paid plugins. Also links to the Marketplace vendor page and GitHub.

Three variants are drafted and judged; the winning one is built.

### D5. Topic structure (per plugin)

We adopt the per-plugin structures proposed in the audits. They are task-oriented, keep the existing file names and IDs (so URLs and the plugins' help links stay stable), and add new topics as new files.

| Plugin | Topics after the overhaul (new topics in bold) |
|---|---|
| AZD | AZD (start); What's new; Install and connect (with the plugin's anchors `connect`, `generate_pat`, `login-from-toolwindow`); Clone and manage projects; Work with pull requests; **Create a pull request**; Review a pull request; **Comment and suggest changes**; **Vote, complete and merge**; **AI assistant** (overview, providers, privacy); Generate titles, descriptions and commit messages; Review pull requests with AI (Chat, Auto Review tour, MCP); Work with pipelines; **Approve pipeline runs**; **Pipeline test results**; **Pipeline metrics and agent pools**; **Edit Azure Pipelines YAML**; **Boards and work items**; Settings reference (anchors `general-configuration`, `ui-customization`, `boards-and-work-items`, `pull-request-filters`); FAQ and troubleshooting; Support |
| JirAI | JirAI (start); What's new (up to 2026.3.16); Installation; Sign in and manage your account; Work with issues; **Create issues**; Issue details; **Field visibility**; Timeline, comments and breakdown; Spaces: summary, list and reports; **Board**; Push Markdown docs to Jira; **AI agent tools and house standard**; **Settings reference**; Remote development; FAQ and known limitations; Support |
| Codecov | Codecov (start); What's new (rewritten to match the changelog); Install and connect; **View coverage in the editor**; **Generate AI prompts**; **Metrics dashboard**; **Folder coverage**; Edit and validate codecov.yml (kept: `Codecov-Usage` id, retitled); **Settings reference**; FAQ and troubleshooting; Support. "Upload a local report" waits for an owner answer. |
| GBrowser | GBrowser (start); Install; Browse the web; **Bookmarks and history**; **Open project files and links**; **DevTools and device emulation**; Settings (reference); **Keyboard shortcuts**; Troubleshooting and support; **What's new** |
| QueryFlag | QueryFlag (start); Install; **Create query templates**; Run a query on selected text; **Settings**; **Troubleshooting**; Support |

### D6. Writing conventions

- **Titles:**
  - Topic `title` is `<Product>: <sentence-case task>`, for example "AZD: Review a pull request". This follows the owner's PR #27 convention for browser tabs and search results.
  - The tree gives a short `toc-title`.
  - Brand casing: AZD, JirAI, GBrowser, Codecov, QueryFlag, Azure DevOps, Jira Cloud.
  - Start pages keep the plain product name.
- **Topic layout:**
  - `<primary-label>`; `<show-structure for="chapter,procedure" depth="2"/>` on long topics.
  - `<tldr>` with the Settings path and shortcut where relevant.
  - The three summaries.
  - `<procedure>` with imperative steps, and `<ui-path>`, `<control>`, `<shortcut>`.
  - `<deflist>` for settings.
  - `<seealso>` with the categories `related` and `external`.
- **Voice:** no "we", no marketing words, present tense, one sentence per source line.
- **Version:** document the latest *released* version. Behaviour that exists only in "Unreleased" changelog entries is not presented as available. Version labels mark only genuinely new sections.
- **Paid plugins:** state the trial length the code and Marketplace show (AZD 10 days, Codecov 10 days, JirAI 7 days, QueryFlag 30 days). Each claim is checked against code or the Marketplace before it is stated.

### D7. Facts, questions and plugin-side findings

- Writers verify against the plugin source in the docs worktrees (`plugins/*/…-docs`, at origin/main).
- Questions only the owner can answer get a conservative default in the docs and go into the final report. Examples:
  - Make JirAI support links point to the public `edgafner/dorkag` issues page plus e-mail.
  - Leave out unconfirmed limits.
- Bugs found in plugins go into a findings report. Plugin product code is not changed. Examples: QueryFlag's token quoting, GBrowser's 5-tab restore limit, Codecov's unused v1 endpoint, AZD's 7 help anchors.

### D8. Delivery

- One docs PR from `feature/docs-overhaul-2026-10`, with commits per area (site-wide, home, each plugin).
- The PR description summarises each plugin and links the open-questions report.
- The owner merges.
- Screenshot tests stay committed locally in each plugin's `feature/doc-screenshots-2026-10` branch.

## 4. Process

1. **Understand.** Done: 9 audits.
2. **Shots, pass 1.** Running: credential-free, one IDE at a time.
3. **Write.** Parallel writers:
   - One per plugin; AZD is split into core and PR/AI. The core writer owns `azdlib.tree` and `Azd.topic`.
   - One site writer owns the shared files.
   - One home-page judge panel.
4. **Integrate.** Reconcile image references with the shot manifests, then build and fix until there are 0 errors.
5. **Review.** Adversarial fact-check per plugin against code, a style check against the guidelines checklist, then a fix round.
6. **Shots, pass 2.** Credentialed, if the tokens arrive, followed by a re-integration.
7. **Deliver.** PR plus the owner report.
