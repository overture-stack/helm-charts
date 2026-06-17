# Helm charts — collaboration conventions

This repo contains Helm charts for the Overture stack. Each chart is an independent deployable unit; changes to one chart do not affect others.

## Session startup

Read `.dev/` in the relevant chart directory before touching that chart's files.

## Chart structure

Each chart follows the standard Helm layout:

```
<chart>/
  Chart.yaml          — name, version, dependencies
  values.yaml         — default values; the public API of the chart
  templates/          — Kubernetes resource templates
  .dev/               — collaboration scaffold (not packaged)
  .helmignore         — excludes .dev/ and other non-chart files from helm package
```

## Publishing

Charts are published as OCI artifacts to `ghcr.io/overture-stack/helm-charts` by Jenkins on every push to `main`. Bump `version` in `Chart.yaml` to trigger a republish; already-published versions are skipped. See `DEVELOPMENT.md` for the full workflow.

## Versioning

- Follow SemVer. Breaking changes to `values.yaml` → minor bump (pre-1.0) or major (post-1.0).
- Bump the version in `Chart.yaml` as part of the same PR as the change.
- Document breaking changes under the relevant chart heading in `README.md`.

## Testing changes locally

```bash
# Render templates without deploying
helm template <release-name> ./<chart-dir> [-f custom-values.yaml]

# Validate chart structure
helm lint ./<chart-dir>

# Update dependencies (required after changing Chart.yaml dependencies)
helm dependency update ./<chart-dir>
```

## Adding or removing env vars

The chart's `deployment.yaml` env section must stay in sync with the app's `.env.schema`. When an env var is added, renamed, or removed in the app, update the chart in the same window of work and document the change.

## Scope and adjacent issues

Stick to the stated scope. Surface design issues verbally, then log them in the chart's `.dev/tech-debt.md`.
