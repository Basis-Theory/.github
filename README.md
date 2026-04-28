# .github

Org-level configuration for the [Basis-Theory](https://github.com/Basis-Theory) GitHub organization.

## Files

| File | Purpose |
|------|---------|
| `renovate-config.json` | Shared Renovate preset extended by every repo's `.github/renovate.json` (`"extends": ["github>Basis-Theory/.github:renovate-config"]`). |
| `PULL_REQUEST_TEMPLATE.md` | Default PR template applied to repos that don't define their own. |

## Renovate

Ducktape runs Renovate on a schedule against the repos enabled in the `/renovate` admin page. Each consumer repo extends this preset, so changes here roll out org-wide on the next Renovate run — no per-repo PR needed.

See [`docs/renovate.md`](https://github.com/Basis-Theory/ducktape/blob/main/docs/renovate.md) in the ducktape repo for the runner architecture and Slack notification flow. Per-rule rationale lives inline in each `packageRule`'s `description` field in `renovate-config.json`.
