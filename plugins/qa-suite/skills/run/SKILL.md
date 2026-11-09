---
name: run
description: Run a QA suite by real clicks on a finished delivery (milestone, release or a delivery plus its bug fixes) before handing it to the QA team. Learns how the QA tests from their recent Jira bugs, builds a scenario matrix of intersections (profile combinations, editions/tenants, cultures, screen widths, Admin vs public site, exports, e-mails, direct POSTs), creates the data through the screens, clicks every row for real, fixes each defect in its own commit (logic fixes start with a failing test), runs a second pass until no row is left untested, and writes the report in the QA's own bug format. Use when the user asks for a QA pass, "QA suite", "test like the QA", "matriz de QA", "suíte de QA", "testar como o QA", "retestar a entrega inteira", or wants zero bugs returned by QA.
argument-hint: "[scope: milestone folder, Jira fixVersion, branch or deliverable list] [--profile=<path>] [--resume]"
---

# QA suite by real clicks

You are doing, **before** the QA team sees the delivery, the same job the QA will do — and doing it
harder. The goal is that the QA finds **zero** bugs. Single-path validation (one happy path per
acceptance criterion) is not enough: QA bugs live in the **intersections** — profile combinations,
old vs new edition/tenant, Admin vs public site, culture A vs culture B, narrow vs wide screen, the
export vs the list, the e-mail vs the screen, the rule enforced on screen but not on the server.

Speak to the user in the language they write in. Write the matrix and report in that language too.

## Non-negotiable rules

1. **Run on finished code.** All implementation of the scope is done first; the suite runs on the
   final branch (top of the stack when branches are stacked). Never interleave with implementation.
2. **Only report what was clicked.** A row is "passed" only when it was executed through the real UI
   in this session, with evidence. "Covered by a unit test" never replaces a click. Never bulk-mark
   rows. If a row was not executed, it is "not tested", with the reason.
3. **Build data the way a user does.** Create records through the application's screens (admin
   "new X", invite, onboarding, forgot-password). SQL/API only for what no screen does (e.g. setting
   a password locally), and every such script ships with its reversal script.
4. **Don't ask the user to prepare test data.** If a row needs a persona or state that does not exist,
   create it (screens first, SQL as last resort) and record it in the report's test-data section.
5. **Check the server, not just the screen.** After every write or refusal, query the database (or
   the API) to confirm what was really stored. Every rule shown on screen is also tried by direct
   POST / with client validation off.
6. **Nothing leaves the machine.** Divert outgoing e-mail to a local pickup folder (or the project's
   equivalent) and read each message. Restore every config you changed at the end.
7. **Use your own headless browser** (Playwright). Do not drive the user's own browser windows unless
   they ask.
8. **Each defect = its own commit.** Logic defects start with a failing automated test, then the fix,
   then green. Presentation defects are confirmed by click, with the justification in the commit
   message. Then the row is clicked again.
9. **Done means "Not tested = 0".** A second pass builds and clicks every row left open. A row may only
   stay open if it is truly impossible locally, stating exactly what is missing.

## Step 0 — Profile

The profile holds everything project-specific. Look for it at `--profile=<path>`, else
`qa-suite/profile.md` at the repository root (or the wrapper root when the project is a multi-repo
wrapper). If it does not exist, create it from [templates/profile.md](templates/profile.md): fill
what you can discover from the codebase (URLs, start scripts, cultures in routes, test command,
DB connection, e-mail settings) and ask the user **only** for what the code cannot tell you (who the
QA is in Jira, the Jira project key). Keep secrets (passwords, tokens) out of the profile — reference
a git-ignored env file instead.

## Step 1 — Scope

Determine what is being handed over:
- the deliverables / cards in scope (if Jira is available, confirm against the **fixVersion /
  release** — cards often join it without going through any local document);
- their functional specs and acceptance criteria;
- the branch and commit range under test;
- previous QA matrices for this scope (their rows are re-executed on the final code).

If `--resume` is passed, read the existing matrix and report and continue from the open rows.

## Step 2 — Learn how the QA tests

Read the bugs the QA opened in the last 60 days (adjust in the profile) — via the Atlassian MCP or
the Jira REST API, reading the full bug description, not only the title. Write the section
"How the QA tests" (see [templates/matrix.md](templates/matrix.md)):

- **Where and with which data:** environment, how they create data (which screens), naming patterns,
  reusable fixtures.
- **Profiles and combinations** they stack on purpose.
- **Editions/tenants, cultures and widths** they compare.
- **How they bypass the screen:** direct POST, client validation off, simulated 500/network failure.
- **Cross-checks:** same record on site × admin × export × e-mail; counter × filtered list; detail ×
  form labels; control case to isolate cause; reproduce twice with different data.
- **How they write a bug** (fields, evidence) — the report reuses this format.
- **Category table** — what they call a bug, with counts.

Turn the patterns into **heuristics**. If there is no Jira history, start from this baseline and
say so:

- Every label/message in every culture; no internal code or English name leaking; no broken accents.
- Draft reopens with everything; incomplete draft saves.
- A refusal points to the field and keeps what was typed.
- Deadline messages carry the date and time.
- Every rule also holds on direct POST.
- Counters follow filters; export equals the list (columns, order, filters, accents).
- Bulk actions report what succeeded and what failed.
- Removed/inactive records don't count and don't receive anything.
- Combined profiles neither skip nor duplicate a step/menu.
- Old edition/tenant data never leaks into the new one (and vice versa).
- Special characters (`<`, `&`, quotes, emoji), long texts, double click, two users at once, stale page.

## Step 3 — Matrix

Write the matrix file (default: `<scope folder>/validation-reports/qa-suite-matrix.md`, or the
location set in the profile) from [templates/matrix.md](templates/matrix.md):

- **One section per deliverable**, every acceptance criterion with at least one row.
- **Previous matrix rows**, re-executed on the final code.
- **New rows:** intersections, lifecycle (create → edit → submit → reopen → remove → re-add),
  neighbouring screens the change can break, concurrency and stale page.
- Columns: `#` · deliverable/AC · profiles/condition · edition · culture · width · origin
  (`AC` / `QA` pattern / `Reg` neighbour regression / `Prev` previous matrix) · what to check ·
  automated test.
- Rows that are pure logic get an automated test (write it); screen rows become the click script.
  Both are kept: the test never replaces the click.

Do not stop for approval unless the user asked to review the matrix first. Show a short summary
(row count per section) and proceed.

## Step 4 — Environment and data

Start the environment per the profile (local is the default; never write to production). Divert
e-mail. Create the personas and states the matrix needs, through the screens, tagging data so it is
easy to find and revert (e.g. `ZZQA <timestamp>`). Record every persona and every SQL with its reversal.

## Step 5 — First pass: click everything

For each row, in each listed culture and width:
- execute it through the UI; screenshot as `<prefix>-<row>-<step>.png` in the evidence folder;
- query the database after each write/refusal;
- open and read every e-mail generated (subject and decoded body, each culture);
- download every export through its button and open it;
- try server rules by direct POST / client validation off;
- for concurrency rows, use two browser contexts at once;
- mark the row: **Passed**, **Defect**, **Question**, or **Not tested** (with reason).

When a defect is found: fix it per rule 8, re-click the row and the rows around it, and log it in the
defects table in the QA's bug format. A defect that exists before this delivery and is not made worse
by it goes to "pre-existing observations" — report it, don't fix it silently.

**Questions:** before raising one, decide whether it is ours to decide (anything the original request
or spec already answers, or a technical choice) — if so, decide, record the decision and move on.
Only genuine product questions remain, each with a concrete example, options A/B and a recommendation.

## Step 6 — Second pass

Review your own report as the QA would. Then:
- build and click every row still "Not tested";
- re-click every row touched by a fix, plus its neighbours;
- add the rows the first pass revealed (new intersections, concurrency);
- re-run the end-to-end script of the scope on the final build;
- run the full automated test suite and record the count before → after.

The second-pass results are the ones that count. Repeat until **Not tested = 0**.

## Step 7 — Report

Write the report (default: next to the matrix, `qa-suite-report.md`) from
[templates/report.md](templates/report.md): summary table, defects found (QA format: current result,
expected result citing the criterion, steps, evidence, commit, severity, deliverable), result per row,
questions, internal decisions, second pass, pre-existing observations, test data with reversal,
tests before → after.

Then give the humans something they will actually read: if the session can publish a visual page
(an artifact), publish a one-page summary (numbers, defects with prints, open questions, next step);
otherwise write a self-contained HTML next to the report. Restore every config changed in step 4,
and end the message with where things stand and what comes next (e.g. "push and hand to QA").
