# Helm charts — agent conventions

This repo contains Helm charts for the Overture stack. Each chart is independently versioned and packaged.

## Session startup

Before working on a chart, read that chart's `.dev/roadmap.md`, `.dev/tech-debt.md`, and `.dev/sessions.md`.

## Repository layout

```
<chart>/
  Chart.yaml          — chart metadata and version
  values.yaml         — default values (the chart's public interface)
  templates/          — Kubernetes manifests
  .dev/               — session notes, roadmap, tech-debt (excluded from helm package)
  .helmignore         — excludes .dev/ from packaging
```

## Chart conventions

- `values.yaml` keys use camelCase. Env var names emitted to containers use UPPER_SNAKE_CASE and must match what the app reads.
- Conditional env vars (optional features, empty-means-disabled) are wrapped in `{{- if }}` blocks.
- Breaking `values.yaml` changes require a minor version bump (pre-1.0) and a README upgrade note.
- The `config.catalogues` list is the primary interface for multi-catalogue Arranger deployments. Catalogue `id` values must be valid DNS labels.

## Versioning

SemVer. Bump `Chart.yaml` version in the same PR as the change. Document breaking changes in `README.md`.

## Testing

```bash
helm template <release-name> ./<chart-dir> [-f custom-values.yaml]
helm lint ./<chart-dir>
```

## Constraints

- No credentials, secrets, or private URLs in any file committed to this repo.
- `.dev/` directories are excluded from packaged charts via `.helmignore` — keep collaboration files there.
