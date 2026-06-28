# Arranger chart — roadmap

## Pending

- **0.5.0 — Rename `config.elasticsearch` to `config.searchEngine`**: the `elasticsearch` key in `values.yaml` and all derived env vars (`ES_HOST`, `ES_USER`, `ES_PASS`, etc.) should be renamed to be search-engine-agnostic. Add a `config.searchEngine.engine` field (string, default `opensearch`) that is passed through as the `SEARCH_ENGINE` env var. Breaking change to `values.yaml` API - warrants a minor bump. Existing deployments will need to rename the key in their overrides.

- **Per-catalogue credentials**: when Arranger implements `process.env[${catalogId}_ES_HOST]` etc. (tracked in Arranger's `localEnvs.ts` TODO), update the chart to emit per-catalogue env vars and document the pattern.

## Done

- **0.4.0** — catalogue-centric refactor: per-catalogue ConfigMaps and volume mounts, env var sync with app schema, deprecated UI sidecar removed, `config.path` made absolute and used as single source of truth for mount path.
