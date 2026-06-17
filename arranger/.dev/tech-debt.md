# Arranger chart — tech debt

## Per-catalogue credentials

The chart currently emits a single global `ES_USER`/`ES_PASS` pair, which all catalogues share. Per-catalogue credentials require Arranger to implement the `${catalogId}_ES_HOST` etc. env var pattern (tracked in `localEnvs.ts` TODO). Once that lands, the chart will need to emit per-catalogue credential env vars and expose them in `values.yaml`.

**Standalone:** no — depends on Arranger app changes first.
**Context:** 0.4.0 catalogue-centric refactor; FIXME in `apps/search-server/src/configs/fromEnv/localEnvs.ts`.
