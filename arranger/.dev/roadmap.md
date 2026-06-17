# Arranger chart — roadmap

## Pending

- **Per-catalogue credentials**: when Arranger implements `process.env[${catalogId}_ES_HOST]` etc. (tracked in Arranger's `localEnvs.ts` TODO), update the chart to emit per-catalogue env vars and document the pattern.

## Done

- **0.4.0** — catalogue-centric refactor: per-catalogue ConfigMaps and volume mounts, env var sync with app schema, deprecated UI sidecar removed, `config.path` made absolute and used as single source of truth for mount path.
