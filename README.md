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
- Renovate merges the PR itself (`platformAutomerge: false`) instead of enabling GitHub's native automerge. Private repositories on the free plan cannot require status checks, so native automerge would merge without waiting for CI.
- Major updates are not opened until approved on the Dependency Dashboard.
- Vulnerability fixes skip the waiting period when GitHub Dependabot alerts are enabled on the repository. OSV is checked as well (direct dependencies only).
- Lock file maintenance runs every Monday before 9am JST and is automerged. Note that `minimumReleaseAge` does not apply to it.
- Go modules are tidied after updates.
- Python runtime minor/major updates (`requires-python`, `.python-version`, the `python` Docker image) are grouped into one "python runtime" PR, opened only after approval on the Dependency Dashboard and never automerged. Otherwise a bump can land in the non-major group for only some of these files and break the lock file update.

## Overrides

Repositories without real CI should set `"automerge": false` in their own `renovate.json`.
