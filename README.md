# .github

Org-level configuration for the [Basis-Theory](https://github.com/Basis-Theory) GitHub organization.

## Files

| File | Purpose |
|------|---------|
| `renovate-config.json` | Shared Renovate preset extended by every repo's `.github/renovate.json` (`"extends": ["github>Basis-Theory/.github:renovate-config"]`). |
| `PULL_REQUEST_TEMPLATE.md` | Default PR template applied to repos that don't define their own. |

## Renovate

Ducktape runs Renovate on a schedule against the repos enabled in the `/renovate` admin page. Each consumer repo extends this preset, so changes here roll out org-wide on the next Renovate run — no per-repo PR needed.

See [`docs/renovate.md`](https://github.com/Basis-Theory/ducktape/blob/main/docs/renovate.md) in the ducktape repo for the runner architecture and Slack notification flow.

### Why digest pinning is skipped for `aquasecurity/*`

The `helpers:pinGitHubActionDigestsToSemver` preset pins every GitHub Action to its commit SHA. To compute that SHA, Renovate calls the GitHub GraphQL API as the action's owning org. The `aquasecurity` GitHub org enforces an IP allow list, and Modal's egress IPs are not on it, so the digest lookup returns:

> Although you appear to have the correct authorization credentials, the `aquasecurity` organization has an IP allow list enabled, and your IP address is not permitted to access this resource.

Renovate logs this at `ERROR` level, which causes it to exit non-zero even when the rest of the repo's run completed successfully. The Modal wrapper then posts a `:warning: Dependency Update Errors` summary to `#dependency-prs`.

The packageRule in `renovate-config.json` disables digest pinning for `aquasecurity/**` only, leaving tag-based version updates intact. If a repo migrates off `aquasecurity/*` actions (e.g. to the shared workflows in [`Basis-Theory/security-workflows`](https://github.com/Basis-Theory/security-workflows)) the rule becomes a no-op but is kept as a guard against re-introduction.
