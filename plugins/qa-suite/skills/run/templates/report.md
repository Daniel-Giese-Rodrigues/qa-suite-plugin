# QA suite report — <scope>

**Date:** … **Matrix:** `qa-suite-matrix.md` (<N> rows).
**Branch:** `<branch>`, from `<commit>` to `<commit>`, pushed: yes/no.
**Environment:** …

**How it was clicked**
- Own headless browser (Playwright); no user window was used.
- Database queried after each write or refusal.
- E-mails diverted to `<folder>` and read one by one (subject and decoded body). Config restored at the end.
- Each export downloaded through its button and opened.

**Evidence:** `<folder>/<prefix>-*.png`
**Automated tests:** from <before> to <after>, all green (`<command>`).

## Summary

| Result | Rows |
|---|---|
| Passed | … |
| Fixed (commit) | … (<n> commits) |
| Question | … |
| Not tested | **0** |
| **Total** | … |

## Defects found (as the QA would open them) — all fixed

| # | Commit | Severity | Current result (before) | Expected (criterion) | Steps | Evidence | Deliverable |
|---|---|---|---|---|---|---|---|
| 1 | `<sha>` | High/Medium/Low | … | … | … | `<print>` | … |

Logic fixes started with a failing test: <list>. Presentation fixes were confirmed by click: <list>.

## Result per row

### <deliverable>

| # | Result | Evidence / note |
|---|---|---|

## Questions (only what is not ours to decide)

| # | Concrete example | Option A | Option B | Recommendation |
|---|---|---|---|---|

## Internal decisions

- …

## Second pass

### New defects (fixed)
### Rows previously not tested, now clicked
### Concurrency and stale page
### End-to-end script re-run on the final build

## Pre-existing observations (outside this delivery, not fixed)

- …

## Test data

| Persona / record | How it was created | Reversal |
|---|---|---|
