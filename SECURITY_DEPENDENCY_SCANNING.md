Dependency scanning and automatic remediation
=========================================

This repository uses a combination of Dependabot and GitHub Actions (and optionally Snyk) to detect and remediate dependency vulnerabilities.

What I added
- Dependabot config: `.github/dependabot.yml` — creates daily PRs for npm and GitHub Actions updates.
- CI scan: `.github/workflows/dependency-scan.yml` — runs `npm audit` on every push and PR and fails the build when vulnerabilities at or above `high` are detected. If you add a `SNYK_TOKEN` secret, the workflow also runs `snyk test` and `snyk monitor`.
- Auto-merge: `.github/workflows/auto-merge-dependabot.yml` — attempts to auto-merge Dependabot PRs when checks pass.

Required repository secrets
- `SNYK_TOKEN` — (optional) API token for Snyk. Add this in the repository Settings → Secrets to enable Snyk scans.

Policies and behavior
- Build failing threshold: `npm audit` is run with `--audit-level=high`. Any `high` or `critical` findings will fail CI.
- Dependabot PRs are created daily; the auto-merge workflow will merge Dependabot PRs if all checks succeed and the PR author is Dependabot.

Recommendations / next steps for the team
- Review and add `SNYK_TOKEN` secret to enable Snyk (recommended for additional coverage).
- Configure branch protection rules to require CI checks (so auto-merge only occurs after passing checks).
- Review Dependabot PRs and set the `open-pull-requests-limit` in `.github/dependabot.yml` if needed.
- If you want stricter thresholds, edit `.github/workflows/dependency-scan.yml` and change `--audit-level` to `moderate` or `low`.

How to disable or tune
- To change update frequency or ecosystems, edit `.github/dependabot.yml`.
- To disable the Snyk job, remove the `SNYK_TOKEN` secret or delete the `snyk` job.

If you want, I can also:
- Add a Dependabot configuration that automatically groups minor/patch updates.
- Add label-based rules or require human review before auto-merge.
