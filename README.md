# qa-suite

A Claude Code plugin that runs a QA suite **by real clicks** on finished code, before the delivery
reaches the QA team. It learns how your QA tests from their recent Jira bugs, builds a matrix of the
intersections where bugs hide (profile combinations, editions, cultures, screen widths, admin vs
site, exports, e-mails, direct POSTs), clicks every row, fixes each defect in its own commit and
does a second pass until nothing is left untested.

## Install

Inside Claude Code:

```
/plugin marketplace add Daniel-Giese-Rodrigues/qa-suite-plugin
/plugin install qa-suite@qa-suite-plugin
```

Update later with `/plugin marketplace update qa-suite-plugin`.

## Use

```
/qa-suite:run <milestone folder | Jira fixVersion | branch>
```

On the first run in a project it creates `qa-suite/profile.md` (environment, personas, cultures,
widths, who the QA is in Jira) and asks only what the code cannot tell.
