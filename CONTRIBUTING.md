# Contributing to pointblank-club/monitoring

Thank you for contributing! This document describes how to contribute, the expected workflow, code style, and testing requirements.

## Table of contents
- How to file issues
- Branching & PR workflow
- Commit message format
- Tests & validation
- PR checklist
- Code review expectations
- Contact & support

## How to file issues
- Use clear, descriptive titles.
- Provide steps to reproduce (if applicable), expected behavior, and actual behavior.
- Include relevant config snippets, logs, or screenshots.
- Tag issues with an appropriate label (bug, enhancement, docs, question).

If opening an alert tuning / dashboard change issue, include:
- Target service or metric
- Proposed threshold or visualization change
- Reason for the change (symptoms, incidents, SLO adjustments)

## Branching & Pull Request workflow
1. Create a branch from `main` named with a short prefix describing the change:
   - feature/<short-description>
   - fix/<short-description>
   - docs/<short-description>
   - ci/<short-description>
2. Make small, focused commits. Prefer one logical change per PR.
3. Push your branch and open a Pull Request targeted at `main`.
4. Use a PR description template where possible. Include:
   - What changed and why
   - Any migration or deployment notes
   - Screenshots of dashboard changes
   - Links to related issues

## Commit message format
We use Conventional Commits. Examples:
- feat(prometheus): add recording rule for http_errors_total
- fix(grafana): correct variable reference in dashboard JSON
- docs: update README with deployment notes

Format: <type>(scope): short description
- type: feat, fix, docs, style, refactor, perf, test, chore
- scope: optional area of the repo

## Tests & validation
Before creating a PR, run these validations locally or rely on CI to run them:
- promtool check rules /prometheus/*.yml (validate Prometheus rules)
- jsonlint or jq --sort-keys to validate dashboard JSON
- YAML syntax checks for k8s manifests
- Lint any custom exporter code (e.g., go vet, golangci-lint, eslint for JS)

Add or update unit tests where appropriate.

## PR checklist
Every PR should include:
- [ ] A clear title and description explaining the change
- [ ] Linked issue(s) if applicable
- [ ] Validation steps and test results
- [ ] Updated or new tests (if applicable)
- [ ] Updated documentation (if behavior or usage changed)
- [ ] Dashboard screenshot(s) if visual change

A reviewer will not merge until checks pass and at least one approval is provided from the observability maintainers.

## Code review expectations
- Reviews should be constructive and focused on correctness, maintainability, and operational safety.
- Reviewers should verify:
  - The change does not introduce noisy alerts
  - Queries are performant and use recording rules when needed
  - Dashboards follow established naming and panel conventions
  - Secrets or sensitive information are not included in the repo

## Alerts & Runbooks
- When creating or changing alerts, add or update the corresponding runbook or playbook describing:
  - How to investigate the alert
  - Common causes
  - Remediation steps
  - Who to notify

## Emergency changes
For emergency fixes (e.g., noisy alert causing paging storms), follow the emergency process:
1. Patch the config in a small PR and mark it as "emergency" in the title
2. Notify the on-call and maintainers via the team's alert channel
3. After fixing the issue, follow up with a postmortem or issue documenting root cause and changes

## Style & conventions
- Prometheus rules: include `summary` and `description` fields for alerts
- Alerts should include runbook links in the `description` when applicable
- Grafana dashboards: use consistent naming and tags; include a short dashboard description
- Keep dashboards and alerts idempotent and reviewable

## Getting help
If you need help:
- Open an issue with a clear description
- Ping the observability maintainers or team in the internal chat
- For urgent issues, follow the emergency process above

Thank you for improving our monitoring. Your contributions help keep PointBlank Club reliable and observable.
