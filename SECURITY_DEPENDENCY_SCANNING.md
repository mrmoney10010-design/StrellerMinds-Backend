Dependency scanning and automatic remediation
=========================================

This repository uses Dependabot plus a GitHub Actions scan to detect dependency vulnerabilities and automatically open remediation PRs.

What I added
- Dependabot config: `.github/dependabot.yml` — creates weekly PRs for `npm` and `github-actions` updates.
- CI scan: `.github/workflows/dependency-scan.yml` — runs `npm audit` on every push and PR and fails the build when vulnerabilities at or above `high` are detected. If a `SNYK_TOKEN` secret is configured the workflow will also run a Snyk test (additional coverage) and fail the job for high/critical findings.

Required repository secrets
- `SNYK_TOKEN` — (optional) API token for Snyk. Add this in the repository Settings → Secrets to enable Snyk scans in CI.

Policies and behavior
- Build failing threshold: `npm audit` is executed with `--audit-level=high`. Any `high` or `critical` findings will cause the CI job to exit non-zero and fail the check.
- Dependabot PRs are created weekly by default. They will include automated labels so contributors can filter and triage them quickly.

Recommendations / next steps for the team
- Add the `SNYK_TOKEN` secret (recommended) to get Snyk's vulnerability intelligence in CI.
- Configure branch-protection rules to require these CI checks on pull requests.
- Review Dependabot PRs and adjust `open-pull-requests-limit` in `.github/dependabot.yml` if you want fewer concurrent updates.
- If you need stricter thresholds change the `--audit-level` value in `.github/workflows/dependency-scan.yml`.

How to disable or tune
- To change update frequency or ecosystems, edit `.github/dependabot.yml`.
- To disable the Snyk job, remove the `SNYK_TOKEN` secret or delete the `snyk` job from the workflow.

If you want, I can also:
- Add grouping rules for Dependabot to combine related updates into a single PR.
- Add an auto-merge workflow for Dependabot PRs (requires branch protection configuration) or create labels and reviewers for PR triage.
