# QA suite profile — <project name>

> Project-specific settings for the `qa-suite` skill. No passwords or tokens here: point to a
> git-ignored env file instead.

## QA team (Jira)

- Jira site: `<your-site>.atlassian.net`
- Project key: `<KEY>`
- QA reporter(s): `<name / accountId>`
- History window: last `60` days of bugs

## Environment

- How to start: `<command or script>`
- How to stop: `<command or script>`
- Admin URL: `http://localhost:<port>`
- Public site URL: `http://localhost:<port>`
- Never test against: `<production URLs>`
- Database for verification: `<server / db name / how to query>`

## Dimensions

- Cultures / languages: `<e.g. /pt-br/, /en-us/>`
- Widths: `1440 / 820 / 390`
- Editions / tenants / years to compare: `<e.g. current and previous edition>`
- Profiles/roles that can be combined: `<list>`

## Personas

| Login | Roles | Notes |
|---|---|---|
| `<login>` | `<roles>` | `<where the password lives: env var / script>` |

## E-mail capture

- How to divert outgoing e-mail locally: `<e.g. SMTP pickup directory in config X>`
- Where messages land: `<folder>`
- What to restore at the end: `<file/setting>`

## Automated tests

- Command: `<test command>`
- Where new tests go: `<project/folder>`

## Output

- Matrix and report folder: `<default: <scope folder>/validation-reports/>`
- Evidence (screenshots) folder: `<git-ignored folder>`
- Human summary: `page` (artifact) or `html` (local file)
