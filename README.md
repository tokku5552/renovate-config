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
- Major updates are not opened until approved on the Dependency Dashboard.
- Vulnerability fixes skip the waiting period when GitHub Dependabot alerts are enabled on the repository. OSV is checked as well (direct dependencies only).
- Lock file maintenance runs every Monday before 9am JST and is automerged. Note that `minimumReleaseAge` does not apply to it.
- Go modules are tidied after updates.

## Overrides

Repositories without real CI should set `"automerge": false` in their own `renovate.json`.
