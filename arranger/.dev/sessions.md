# Arranger chart — sessions

Newest first.

---

## 2026-06-17

**Done:**

- Full 0.4.0 overhaul:
  - `values.yaml`: replaced `config.components.*` with `config.catalogues[]`; removed top-level `*Configs` fallbacks; removed deprecated `apiConfig`, `apiImage`, `apiIngress`, `uiConfig`, `uiImage`, `uiIngress` blocks; renamed `config.port` → `config.serverPort`, `config.maxDownloadRows` → `config.downloadMaxRows`; updated `config.path` default to `/app/configs`; added `config.features.*`, `config.graphql.*`, `config.elasticsearch.setsIndex/setsType`, `config.allowedCorsOrigins`, `config.pingMs`, `config.searchEngine`.
  - `deployment.yaml`: fixed all env var name mismatches (`CONFIG_PATH`→`CONFIGS_PATH`, `PORT`→`SERVER_PORT`, `DEBUG`→`ENABLE_DEBUG`, `MAX_DOWNLOAD_ROWS`→`DOWNLOAD_MAX_ROWS`, `PING_PATH` ref fixed); removed dead vars (`DOCUMENT_TYPE`, `ES_INDEX`); added 12 missing env vars; per-catalogue volume mounts replacing single flat mount; UI sidecar container removed; image refs simplified (dropped `apiImage` coalesces); `config.path` used for mount path.
  - `configmap-server-configs.yaml`: rewritten to generate one ConfigMap per catalogue with deduplication guard.
  - `ingress-server.yaml`: removed `apiIngress` ternary — uses `ingress` directly.
  - Deleted `configmap-nginx-conf.yaml` and `ingress-ui.yaml`.
  - Devctx scaffolded: repo-level `CLAUDE.md`/`AGENTS.md`, per-chart `.dev/` and `.helmignore`.
  - `Chart.yaml`: version bumped to 0.4.0.
  - `README.md`: 0.4.0 upgrade notes added.

- Catalogue config key rename: `components.*` → `configs.*`, `baseConfigs` → `base`, `extendedConfigs` → `extended`, `facetsConfigs` → `facets`, `matchboxConfigs` → `matchbox`, `tableConfigs` → `table` — applied in `values.yaml`, `configmap-server-configs.yaml`, `README.md`.
- Env vars in `deployment.yaml` sorted alphabetically; category comments removed.
- Terraform migration noted in tech-debt.
- OCI publishing setup: `Jenkinsfile`, `DEVELOPMENT.md` added to repo root; `README.md` and `CLAUDE.md` publishing sections updated. Pipeline uses `jenkins-pipeline-library@add-softeng-charts` (temporary — update to `main` once that branch merges).

**Open threads:**

- Per-catalogue credentials (awaiting Arranger app support — see roadmap).
- Terraform values migration to 0.4.0 key names (see tech-debt).
