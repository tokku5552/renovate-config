# renovate-config

Shared Renovate preset for tokku5552's repositories.

## Usage

Put this in each repository's `renovate.json`:

```json
{
  "extends": ["local>tokku5552/renovate-config"]
}
```

The Mend Renovate App must be installed on this repository as well as on the repository that extends it.

## What it does

- Non-major updates are grouped into one PR and automerged after a 7-day `minimumReleaseAge`. Automerge only happens when the repository's own status checks pass; PRs without checks stay open.
- Update branches are created only on Monday 00:00-05:59 (Asia/Tokyo). Without a schedule, the group PR was opened again every time one more dependency passed the 7-day age, which meant a PR and a CI run almost every day in repositories with fast-moving modules such as `google.golang.org/api`. Automerge is not limited to this window, so a PR whose checks finish later still merges. Vulnerability fixes ignore the schedule (Renovate always does this for `vulnerabilityAlerts`).
- Renovate merges the PR itself (`platformAutomerge: false`) instead of enabling GitHub's native automerge. Private repositories on the free plan cannot require status checks, so native automerge would merge without waiting for CI.
- Major updates are not opened until approved on the Dependency Dashboard.
- Vulnerability fixes skip the waiting period when GitHub Dependabot alerts are enabled on the repository. OSV is checked as well (direct dependencies only).
- Lock file maintenance is off. Its updates carry no release timestamp, so since Renovate 42 (`minimumReleaseAgeBehaviour=timestamp-required`) the `renovate/stability-days` check stays pending forever and the PR never automerges. Turning `minimumReleaseAge` off for it would let unaged transitive versions in, so it is disabled instead.
- Go modules are tidied after updates.
- Python runtime minor/major updates (`requires-python`, `.python-version`, the `python` Docker image) are grouped into one "python runtime" PR, opened only after approval on the Dependency Dashboard and never automerged. Otherwise a bump can land in the non-major group for only some of these files and break the lock file update.

## Overrides

Repositories without real CI should set `"automerge": false` in their own `renovate.json`.
