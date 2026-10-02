# DORKAG docs overhaul: implementation plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task by task. Steps use checkbox (`- [ ]`) syntax for tracking.
> In this session the plan is executed through Workflow orchestration (ultracode): one subagent per task, with independent review agents.

**Goal:** Make the DORKAG Writerside site accurate, complete and well illustrated for AZD, JirAI, GBrowser, Codecov and QueryFlag, and turn the home page into a proper starting page.

**Architecture:**
- Per-plugin writer agents rewrite topics in place, keeping the IDs, and add new topics.
- A site writer owns the shared configuration.
- Screenshot agents run IDE Starter UI tests in each plugin repo, one IDE at a time, and export light and dark PNGs.
- An integration step reconciles image references, builds and fixes.
- Review agents fact-check against the plugin source.

**Tech stack:**
- Writerside XML topics, built by `jetbrains/writerside-builder:2026.09.0357` (docker) and checked by `wrs-doc-app.jar`.
- UI tests: IntelliJ IDE Starter and Driver SDK (Kotlin), Gradle `--no-scan`.
- Image optimisation: `oxipng`.

**Spec:** `docs/superpowers/specs/2026-10-02-docs-overhaul-design.md`

## Global constraints

- Builder `2026.09.0357`. The build must report 0 errors, and the checker exits 0.
- Topic file names and IDs are never renamed or removed; new topics are new files. URLs and the plugins' help links depend on them.
- Titles take the form `<Product>: <sentence-case task>` with a short `toc-title`. Brand casing: AZD, JirAI, GBrowser, Codecov, QueryFlag.
- Every topic has `<link-summary>`, `<card-summary>` and `<web-summary>`.
- Screenshots:
  - Islands Light `name.png` plus Islands Dark `name_dark.png`, 1280×800 window, 2x (offscreen paint helper), tight crops.
  - `width` equals half the pixel width, capped at 706 (use `thumbnail="true"` above that), with `border-effect="rounded"`.
  - Names are globally unique across `Dorkag/images/**`.
- Document the latest released plugin version. Never present "Unreleased" changelog items as available.
- No real names, e-mails, tokens or private hostnames in any image or text.
- Credentials are never searched for or read. Live-data shots use local mock backends with neutral demo data.
- Gradle always runs with `--no-scan`. UI tests only run through `docshots.sh` (the screen lock).
- Paths:
  - Docs: `~/IdeaProjects/edgafner/edgafner.github.io-docs`.
  - Scratch: `$SCRATCH` = `<session scratchpad>`.

## Review focus

The failure modes most likely to hurt a reader, and the check each task must pass:

1. **In-IDE help links land on a missing anchor.** AZD links to `#generate_pat`, `#login-from-toolwindow`, `#connect`, `#general-configuration`, `#boards-and-work-ites`, `#ui-customization` and `#pull-request-filters`. Check: grep the built HTML for every anchor (Task 8, step 4).
2. **A light screenshot shows on a dark page, or a dark twin is missing.** Check: `check-images.py` fails when a new image has no `_dark` twin (Task 8).
3. **An old URL breaks.** Check: the topic-ID list before and after must be a superset (Task 8).
4. **An image name collides with a root file, so the wrong image ships silently.** Check: duplicate-basename scan (Task 8).
5. **Docs describe unreleased behaviour, or wrong UI labels.** Check: adversarial fact-check against code and the released changelog (Task 9).

---

### Task 0: Verification scripts (shared by all tasks). DONE.

**Files (created and run on the baseline):**
- `$SCRATCH/verify/check-images.py`. Exits 1 on any of:
  - `MISSING`: a referenced `src` is not found.
  - `DUPLICATE`: the same basename appears twice under `Dorkag/images`.
  - `NO_DARK`: a PNG was added since 59875f3 without a `_dark` twin.
  - `WIDTH`: a `width` attribute is larger than the pixel width.
  - `WRONG_ALT`: the alt text is a file name.

  `-v` lists unreferenced images.
- `$SCRATCH/verify/check-ids.sh`. Fails if any topic ID present at baseline 59875f3 is gone.
- `$SCRATCH/verify/check-xml.sh`. Runs xmllint on every topic, tree, list and cfg file.
- `$SCRATCH/site-build/build.sh [src] [name]`. Verified locally. It replicates CI's builder (`helpbuilderinspect`, `-product Dorkag/dorkag`) and the checker jar.

**Baseline result:**
- `check-images`: 74 errors. These are the DUPLICATE folders `images/azd` and `images/codecov`, and file-name alts. Task 2 and the writers fix them.
- `check-ids`: 43 topic IDs, OK.
- `check-xml`: OK.
- Build: 0 errors, checker exit 0.

### Task 1: Credential-free screenshots (running: workflow `docs-screenshots-credfree`)

**Files:**
- Create in each plugin worktree (`plugins/*/…-docs`):
  - `src/uiTest/kotlin/<pkg>/docshots/DocScreenshots.kt`
  - `src/uiTest/kotlin/<pkg>/<Plugin>DocScreenshotsUI.kt`
  - A Setup pin change to `IU-263.6259.32`.
- Create: `Dorkag/images/<plugin>/<name>.png` plus `<name>_dark.png`, and `$SCRATCH/shots/<plugin>-manifest.json`.

**Interfaces:**
- Produces: manifest entries `{name, has_dark, width, height, shows, alt, target_topic, target_section, replaces_old_image}`. Task 8 consumes them.

- [ ] Run order: GBrowser, QueryFlag, Codecov, AZD, JirAI. One agent per plugin, each using `zsh $SCRATCH/docshots/docshots.sh <worktree> '*<Plugin>DocScreenshotsUI*' light|dark <out>`.
- [ ] Every PNG is reviewed visually by the agent; bad shots are re-run.
- [ ] Commit the test code on the local `feature/doc-screenshots-2026-10` branch. Never push.

### Task 2: Site-wide configuration (site writer)

**Files:**
- Modify:
  - `.github/workflows/build-docs.yml` (`DOCKER_VERSION: '2026.09.0357'`)
  - `CLAUDE.md` and `README.md` (builder version, the working docker command from `build.sh`, jirai in the structure tree)
  - `.gitignore` (`*.log`, `.DS_Store`, `.vscode/`)
  - `Dorkag/writerside.cfg` (`<settings><smart-ignore-vars>true</smart-ignore-vars></settings>`)
  - `Dorkag/cfg/buildprofiles.xml` (`<llms-txt/>` and `<sitemap priority="0.35" change-frequency="monthly"/>` as direct children of `<buildprofiles>`; `<product-web-url>https://edgafner.github.io/</product-web-url>`; remove the `<shortcuts>` block; layout names; a download button for codecovlib and queryflaglib only if the URLs are verified)
  - `Dorkag/labels.list` (names "AZD", "QueryFlag"; versions AZD V2026.3.6, Codecov V2026.3.2, JirAI V2026.3.16)
  - `Dorkag/c.list` (add `related`)
- Delete:
  - `Dorkag/topics/common-support.snippet`, but only if nothing includes it.
  - Every file in `Dorkag/images/azd/` and `Dorkag/images/codecov/` whose basename also exists in `Dorkag/images/`. Compute the list; never delete files created today.

- [ ] **Step 1:** Make the edits.
- [ ] **Step 2:** Run `xmllint --noout` on every changed XML file. Expect no output.
- [ ] **Step 3:** Run `bash $SCRATCH/site-build/build.sh ~/IdeaProjects/edgafner/edgafner.github.io-docs site`. Expect the checker to exit 0, and `llms.txt` plus `sitemap.xml` to exist in `$SCRATCH/site-build/site-site/`.
- [ ] **Step 4:** Commit: `Upgrade Writerside builder to 2026.09.0357 and enable llms.txt and sitemap`.

### Task 3: Home page (judge panel)

**Files:**
- Modify: `Dorkag/topics/Dorkag.topic`.
- Create: `Dorkag/images/gbrowser_icon.svg`, copied from `plugins/gbrowser/gb-docs/src/main/resources/META-INF/pluginIcon.svg` (check that the name is unused).

- [ ] **Step 1:** Three drafters write competing `<section-starting-page>` versions:
  - A: a catalogue of cards;
  - B: task-first ("what do you want to do");
  - C: what's new and install first.
  Each must be valid XML and use only existing topics and icons.
- [ ] **Step 2:** A judge scores the drafts on guideline compliance (cookbook 3.9), accuracy, clarity and consistency with the plugin start pages. It writes the final topic from the winner, grafting the best parts of the others.
- [ ] **Step 3:** `xmllint --noout`, then build. Expect 0 errors.
- [ ] **Step 4:** Commit: `Turn the home page into a Writerside starting page`.

### Tasks 4–6: Plugin writers (parallel)

Each writer owns only its own files, listed below.

**Inputs for every writer:**
- The spec;
- `$SCRATCH/understand/guidelines.md`;
- its audit report(s) in `$SCRATCH/understand/`;
- the plugin source worktree;
- `$SCRATCH/shots/<plugin>-manifest.json` when it exists.

| Task | Writer | Owns | New topic files (exact names) |
|---|---|---|---|
| 4a | AZD core | `Dorkag/azdlib.tree`, `Azd.topic`, `Azd-Whats-New`, `Azd-Installation`, `Azd-Manage-projects-hosted-on-Azure-DevOps`, `Azure-DevOps-Pipelines`, `Azd-Settings-Configuration`, `Azd-FAQ`, `Azd-Reference-And-Support` | `Azd-Pipeline-Approvals.topic`, `Azd-Pipeline-Tests.topic`, `Azd-Pipeline-Metrics.topic`, `Azd-Pipeline-Yaml.topic`, `Azd-Boards.topic` |
| 4b | AZD PRs and AI | `Work-with-Azure-DevOps-pull-requests`, `Pull-Request-Details`, `AI-Title-Description-Generation`, `AI-Pull-Request-Review` | `Azd-Create-Pull-Request.topic`, `Azd-PR-Comments.topic`, `Azd-PR-Complete.topic`, `Azd-AI.topic` |
| 5 | JirAI | `Dorkag/jirailib.tree`, `Dorkag/topics/jirai/*` | `Jira-Create-Issues.topic`, `Jira-Field-Visibility.topic`, `JirAI-Board.topic`, `JirAI-Agent-Tools.topic`, `JirAI-Settings-Reference.topic` |
| 6a | Codecov | `Dorkag/codecovlib.tree`, `Dorkag/topics/codecov/*` | `Codecov-Editor-Coverage.topic`, `Codecov-AI-Prompts.topic`, `Codecov-Metrics.topic`, `Codecov-Folder-Coverage.topic`, `Codecov-Settings-Reference.topic` |
| 6b | GBrowser | `Dorkag/gbrowserlib.tree`, `Dorkag/topics/gbrowser/*` | `GBrowser-Whats-New.topic`, `GBrowser-Bookmarks-and-History.topic`, `GBrowser-Open-Files-and-Links.topic`, `GBrowser-DevTools-and-Device-Emulation.topic`, `GBrowser-Keyboard-Shortcuts.topic` |
| 6c | QueryFlag | `Dorkag/queryflaglib.tree`, `Dorkag/topics/queryflag/*` | `QueryFlag-Create-Templates.topic`, `QueryFlag-Settings.topic`, `QueryFlag-Troubleshooting.topic` |

The AZD tree order is:
- AZD
  - What's new
  - Install and connect
  - Clone and manage projects
  - Work with pull requests
    - Create (`Azd-Create-Pull-Request`)
    - Review (`Pull-Request-Details`)
    - Comment (`Azd-PR-Comments`)
    - Vote and complete (`Azd-PR-Complete`)
  - AI assistant (`Azd-AI`)
    - Generate titles, descriptions and commits (`AI-Title-Description-Generation`)
    - Review with AI (`AI-Pull-Request-Review`)
  - Work with pipelines
    - Approvals
    - Tests
    - Metrics and agent pools
    - YAML
  - Boards and work items
  - Settings reference
  - FAQ
  - Support

**Steps for every writer:**
- [ ] **Step 1:** Read the inputs. List every MISSING or OUTDATED feature and every inaccuracy from the audit.
- [ ] **Step 2:** Rewrite or create the owned topics following the spec D5 and D6 and the guideline cookbook. Verify each UI label, Settings path, shortcut and default against the source:
  - message bundles `*.properties`;
  - `plugin.xml`;
  - configurables;
  - settings state classes;
  - keymaps.
- [ ] **Step 3:** Images:
  - Reference manifest images first.
  - Then planned shot-list names; return them as `planned_images`.
  - Then old images with audit verdict `ok`.
  - Never reference an image with verdict `wrong-content`.
- [ ] **Step 4:** Run `xmllint --noout` on every owned file. Expect no output.
- [ ] **Step 5:** Return:
  - topics changed or created;
  - `planned_images`;
  - unverifiable facts;
  - owner questions;
  - plugin-side bugs found.
- [ ] Commits are made by the orchestrator per plugin after integration (Task 8), so parallel writers never race on git.

### Task 7: Image cleanup

- [ ] **Step 1:** After Tasks 1–6, list the images that no topic references and that were not added today.
- [ ] **Step 2:** `git rm` them. Git history keeps them.
- [ ] **Step 3:** Run `check-images.py`. Expect no DUPLICATE and no MISSING lines.

### Task 8: Integration (orchestrator)

- [ ] **Step 1:** For each `planned_images` entry, look for it in the manifest:
  - If it exists, keep the reference and set `width` from the PNG (`sips -g pixelWidth`): `min(px, 706)`, plus `thumbnail="true"` when px > 706.
  - If it is missing, fall back to the old image only if its audit verdict is `ok` or `stale-UI` and the concept still matches. Otherwise remove the `<img>`.
- [ ] **Step 2:** Run `check-images.py` and `check-ids.sh`. Expect both to exit 0.
- [ ] **Step 3:** Run `build.sh`. Expect 0 errors and the checker to exit 0. Fix anything else.
- [ ] **Step 4:** Anchors: `grep -l 'id="generate_pat"' $SCRATCH/site-build/site-*/azd-installation.html`, and the same for each plugin HelpId anchor. Expect a hit for each.
- [ ] **Step 5:** Commit per area: site, home, AZD, JirAI, Codecov, GBrowser, QueryFlag.

### Task 9: Review (parallel adversarial agents)

- [ ] **Step 1:** Per plugin, a fact-checker reads every changed topic and tries to refute each claim against the code and the released changelog. It returns a list of wrong claims with evidence.
- [ ] **Step 2:** A style reviewer runs the guideline checklist (guidelines.md section 6) over all changed topics.
- [ ] **Step 3:** A visual reviewer opens every new PNG and its `_dark` twin: correct state, no personal data, no clutter.
- [ ] **Step 4:** A fixer applies the confirmed findings. Re-run the Task 8 steps 2–4 checks.
- [ ] **Step 5:** Commit: `Fix review findings`.

### Task 10: Credentialed screenshots (only when the owner provides tokens)

- [ ] Same harness as Task 1, live-data shots only (AZD, Codecov, JirAI).
- [ ] Then repeat Task 8 steps 1–5 for the affected topics.

### Task 11: Delivery

- [ ] **Step 1:** `git push -u origin feature/docs-overhaul-2026-10`.
- [ ] **Step 2:** `gh pr create` with a per-plugin summary, before/after image samples, and the owner-questions report. End the description with the attribution line.
- [ ] **Step 3:** Send the owner report (open questions, plugin-side bugs, credentialed shots pending) with SendUserFile.
