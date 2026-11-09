# QA suite — <scope>

**Goal:** the QA team finds no bug in this delivery.
**Code:** branch `<branch>`, from `<first commit>` to `<last commit>`. Deliverables: `<list>`.
**Environment:** Admin `<url>`, Site `<url>`, database `<db>`, editions `<x, y>`.
**Widths:** 1440 / 820 / 390. **Cultures:** `<a, b>`.
**Results:** `qa-suite-report.md`.

## 1. How the QA tests (read from Jira)

Source: `<N>` bugs opened by `<QA>` in the last `<D>` days.

**Where and with which data**
- …

**Profiles and combinations**
- …

**Editions, cultures and widths**
- …

**How they bypass the screen**
- …

**Cross-checks they make**
- …

**How they write a bug:** …

**What they call a bug**

| Category | Bugs |
|---|---|
| … | … |

**Heuristics applied here (each became rows)**
- …

## 2. Matrix

Columns: **#** · **Deliverable/AC** · **Profiles / condition** · **Ed.** · **Cult.** · **Width** ·
**Origin** (AC = acceptance criterion; QA = QA pattern; Reg = regression in a neighbouring screen;
Prev = row from an earlier matrix, re-executed) · **What to check** · **Automated test**.

Base personas: …
Created by this suite through the screens: …

### 2.1 Acceptance criteria — <deliverable>

| # | AC | Profiles / condition | Ed. | Cult. | Width | Origin | What to check | Test |
|---|---|---|---|---|---|---|---|---|
| D1-01 | … | … | … | … | 1440/820/390 | AC | … | no |

### 2.N Previous matrix rows, re-executed on the final code

| # | … |
|---|---|

### 2.N+1 New rows (intersections, lifecycle, neighbourhood, concurrency)

| # | … |
|---|---|
