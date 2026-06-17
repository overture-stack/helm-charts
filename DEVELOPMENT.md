# Development Guide

Local setup, workflows, and conventions for the `helm-charts` repository.

## Prerequisites

- [Helm](https://helm.sh/docs/intro/install/) v3.8+

## Repository structure

```
helm-charts/
├── <chart-name>/  # One directory per chart (Helm chart source)
│   └── .dev/      # Per-chart working documents (roadmap, tech-debt, session log)
├── CLAUDE.md      # Claude Code conventions and session discipline
├── AGENTS.md      # Agent conventions (non-Claude)
├── DEVELOPMENT.md # This file
└── README.md      # Upgrade notes and end-user usage instructions
```

## Developer workflow

Push chart source changes to `main`; Jenkins handles linting, packaging, and publishing.

1. Create or modify chart source in a directory at the repo root
2. Lint locally: `helm lint <chart-name>`
3. Bump `version` in `<chart-name>/Chart.yaml` for any change to an existing chart
4. Push to `main`

Jenkins will package and publish the chart to GHCR. The new version is available immediately after the pipeline completes.

## Adding a new chart

```shell
helm create <chart-name>   # Scaffold chart source
helm lint <chart-name>     # Validate locally before pushing
# Set version in <chart-name>/Chart.yaml
git add <chart-name>/
git commit -m "add <chart-name> chart"
git push
```

## Local chart testing

Package a chart and inspect the archive before pushing:

```shell
# Render templates with default values (dry-run)
helm template <release-name> ./<chart-name>

# Render templates with custom values
helm template <release-name> ./<chart-name> -f custom-values.yaml

# Validate chart structure
helm lint ./<chart-name>

# Update dependencies (required after changing Chart.yaml dependencies)
helm dependency update ./<chart-name>

# Package and inspect the archive
helm package <chart-name>
tar -tzf <chart-name>-<version>.tgz
helm show all ./<chart-name>-<version>.tgz
```

Do not commit `.tgz` files to `main`.

## Using this repository's charts

```shell
helm install <release-name> oci://ghcr.io/overture-stack/helm-charts/<chart-name> --version <version>
```

Charts are published as OCI artifacts to `ghcr.io/overture-stack/helm-charts`. No `helm repo add` step is required.

## Jenkins CI

The Jenkins pipeline (`Jenkinsfile`) runs on every push to `main` and:

1. Scans top-level directories for Helm chart sources (`Chart.yaml` required)
2. Checks GHCR for each chart — skips versions already published
3. Lints (`helm lint`) and packages (`helm package`) new versions
4. Pushes to `oci://ghcr.io/overture-stack/helm-charts` via `helm push`

Bumping `version` in `Chart.yaml` is the mechanism for triggering a republish.

## Working documents

Each chart has a `.dev/` directory for chart-specific working documents. The repo root also has a `.dev/` for cross-chart concerns (CI, tooling, repo structure).

| File           | Purpose                                          |
| -------------- | ------------------------------------------------ |
| `roadmap.md`   | Planned work                                     |
| `tech-debt.md` | Known issues logged during development           |
| `sessions.md`  | Session log, newest first                        |

Keep these current when making changes — they are the project's institutional memory.

## AI tooling

This repo uses Claude Code for development assistance. See `CLAUDE.md` for Claude-specific conventions and session discipline. See `AGENTS.md` for conventions that apply to any AI agent.
