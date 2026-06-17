# Overture Helm-charts

Repository for Helm charts used by the Overture stack.

## Using these charts

```shell
helm install <release-name> oci://ghcr.io/overture-stack/helm-charts/<chart-name> --version <version>
```

Charts are published as OCI artifacts to `ghcr.io/overture-stack/helm-charts`. No `helm repo add` step is required.

## Publishing

Bump `version` in `<chart-name>/Chart.yaml` and push to `main`. Jenkins lints, packages, and publishes automatically. See [DEVELOPMENT.md](DEVELOPMENT.md) for the full workflow and local testing commands.

---

# Upgrade notes

## Arranger

### Version 0.4.0 — breaking changes

This release aligns the chart with the current Arranger search-server and introduces multi-catalogue support.

**Multi-catalogue config (replaces `config.components.*`):**

Config files are now per-catalogue. Replace `config.components.*` with a `config.catalogues` list:

```yaml
# Old (pre-0.4.0)
config:
  components:
    baseConfigs: { ... }
    tableConfigs: { ... }

# New
config:
  catalogues:
    - id: my-catalogue   # DNS label: lowercase alphanumeric and hyphens only
      configs:
        base: { ... }
        table: { ... }
```

A single-catalogue deployment is one entry in the list. Each catalogue's config files are mounted at `<config.path>/<id>/` and discovered automatically by the server.

**Deploying with Terraform:**

Pass config files as decoded JSON objects — the chart's `toPrettyJson` step requires objects, not raw strings.

```hcl
values = [
  file("helm/arranger-my-catalogue/values.yaml"),
  jsonencode({
    config = {
      catalogues = [
        {
          id = "my-catalogue"
          configs = {
            extended = jsondecode(file("./helm/arranger-my-catalogue/configs/extended.json"))
            facets   = jsondecode(file("./helm/arranger-my-catalogue/configs/facets.json"))
            table    = jsondecode(file("./helm/arranger-my-catalogue/configs/table.json"))
          }
        }
      ]
    }
  })
]
```

The `id` becomes the ConfigMap name suffix (`<release>-server-configs-<id>`) and must be a valid DNS label. For multiple catalogues, add additional entries to the `catalogues` list with distinct `id` values.

**Migrating from pre-0.4.0 Terraform:**

The old pattern passed raw string file contents under top-level keys:

```hcl
# Old — no longer works in 0.4.0
jsonencode({
  "extendedConfigs" = file("./configs/extended.json")
  "facetsConfigs"   = file("./configs/facets.json")
  "tableConfigs"    = file("./configs/table.json")
})
```

Replace with the `config.catalogues` structure above, wrapping each `file(...)` call with `jsondecode`.

**Removed values:**

| Removed | Replacement |
|---|---|
| `config.documentType` | Set `documentType` in the catalogue's `base.json` |
| `config.elasticsearch.index` | Set `esIndex` in the catalogue's `base.json` |
| `config.components.*` | `config.catalogues[].configs.*` |
| `baseConfigs`, `extendedConfigs`, `facetsConfigs`, `matchboxConfigs`, `tableConfigs` (top-level fallbacks) | `config.catalogues[].configs.*` |
| `apiConfig.*`, `apiImage.*`, `apiIngress.*` | `config.*`, `image.*`, `ingress.*` |
| `uiConfig.*`, `uiImage.*`, `uiIngress.*` | UI sidecar removed |

**Renamed values:**

| Old | New |
|---|---|
| `config.port` | `config.serverPort` |
| `config.maxDownloadRows` | `config.downloadMaxRows` |

**New values:**

| Value | Description |
|---|---|
| `config.catalogues[]` | List of catalogues; each requires a DNS-label `id` and optional `configs.*` |
| `config.features.*` | Feature flags (`disableDownloads`, `disableFilters`, `disableGraphqlPlayground`, `disableSets`, `allowCustomDownloadMaxRows`) |
| `config.graphql.maxAliases` / `maxDepth` | GraphQL security limits; leave unset for no limit |
| `config.elasticsearch.setsIndex` / `setsType` | Arranger sets index configuration |
| `config.allowedCorsOrigins` | List of allowed CORS origins; omit to allow all |
| `config.pingMs` | Health check ping interval in milliseconds |
| `config.searchEngine` | `elasticsearch` or `opensearch`; leave empty to auto-detect |

**`config.path` is now absolute (`/app/configs`)** and is used as both the `CONFIGS_PATH` env var and the container mount path. Override if your image uses a different location.

### Version 0.3.1

all `api<values>` are now just `<values>`:

Old:

    apiConfig:
      ...

    apiImage:
      ...

    apiIngress:
      ...

New:

    config:
      ...

    image:
      ...

    ingress:
      ...

also, new required config values:
config:
documentType: "" (i.e. graphql field; e.g. "file")
index: "" (i.e. Elasticsearch index/alias to query)

Version: 0.3.0

Values for ingress config changed:

Old:

    apiIngress:
      enabled: true
      host: host.local

    uiIngress:
      enabled: true
      host: host.local

New:

    apiIngress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

    uiIngress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## DMS-ui

Version: 1.1.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Ego

Version: 3.1.0

Old:

    ingressApi:
      enabled: true
      host: host.local

    ingressUi:
      enabled: true
      host: ui.host.local

New:

    ingressApi:
      enabled: true
      hosts:
        - host: ego.local
          paths:
            - path: /api/(.*)
              pathType: ImplementationSpecific

    ingressUi:
      enabled: true
      hosts:
        - host: ui.ego.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Lectern

Version: 0.7.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Maestro

See examples on how to use this chart in ./\_examples

Version: 0.9.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

### Muse

Version: 0.10.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

### Rollcall

Version: 1.5.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Score

Version: 0.12.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Song

Version: 0.12.3

Allows using your own existing postgres instance.
Set `useInternalDep` as `false` to prevent starting a "song-postgres" pod.

New params: (and their default values)

    postgres:
      host: "song.fullname"-postgres.
      port: 5432
      secret:
        name: "song.fullname"-postgres
        key: postgres-password
      useInternalDep: true

Version: 0.12.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific

## Stateless-svc

Version: 0.1.0

Old:

    ingress:
      enabled: true
      hosts:
      - host.local

New:

    ingress:
      enabled: true
      hosts:
        - host: host.local
          paths:
            - path: /
              pathType: ImplementationSpecific
