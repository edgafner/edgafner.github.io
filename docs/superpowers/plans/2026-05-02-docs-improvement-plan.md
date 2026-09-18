# Documentation Improvement Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Improve documentation for azd, codecov, gbrowser, and queryflag plugins using Writerside best practices and updating versions/visuals.

**Architecture:** Systematic iteration per plugin. Centralize common sections using snippets. Refactor lists to procedures.

**Tech Stack:** Writerside (XML Semantic Markup), JetBrains Plugin Environment.

---

### Task 1: Global Updates & Centralization

- [ ] **Step 1: Update labels.list**
Update `Dorkag/labels.list` with new versions:
  - azd: `V2026.1.45`
  - codecov: `V2026.1.13`
  - gbrowser: `V2026.1.8`
  - queryflag: `V2026.1.3`

- [ ] **Step 2: Create Common Support Snippet**
Create `Dorkag/topics/common-support.snippet` with standard reference/support info.

- [ ] **Step 3: Update Reference topics to use snippet**
Update `*-Reference-and-Support.topic` in all 4 plugin directories to include the new snippet.

### Task 2: Azd Plugin Iteration
(Repeat similar tasks for Codecov, GBrowser, QueryFlag)

- [ ] **Step 1: Convert lists to <procedure>** in `Azd-Installation.topic` and others.
- [ ] **Step 2: Create Azd-Whats-New.topic**.
- [ ] **Step 3: Screenshot Audit** (Compare current images with local repo state provided by user).
- [ ] **Step 4: Update Azd navigation tree** (`azdlib.tree`).

### Task 3: Codecov Plugin Iteration
- [ ] **Step 1: Convert lists to <procedure>**.
- [ ] **Step 2: Create Codecov-FAQ.topic** and `Codecov-Whats-New.topic`.
- [ ] **Step 3: Screenshot Audit**.
- [ ] **Step 4: Update Codecov navigation tree** (`codecovlib.tree`).

### Task 4: GBrowser Plugin Iteration
- [ ] **Step 1: Convert lists to <procedure>**.
- [ ] **Step 2: Create GBrowser-FAQ.topic** and `GBrowser-Whats-New.topic`.
- [ ] **Step 3: Screenshot Audit**.
- [ ] **Step 4: Update GBrowser navigation tree** (`gbrowserlib.tree`).

### Task 5: QueryFlag Plugin Iteration
- [ ] **Step 1: Convert lists to <procedure>**.
- [ ] **Step 2: Create QueryFlag-Settings.topic**, `QueryFlag-FAQ.topic`, and `QueryFlag-Whats-New.topic`.
- [ ] **Step 3: Screenshot Audit**.
- [ ] **Step 4: Update QueryFlag navigation tree** (`queryflaglib.tree`).
