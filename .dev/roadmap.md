# Helm-charts repo — roadmap

## Pending

- **Per-chart CHANGELOG.md**: replace the upgrade-notes section in the root `README.md` with a dedicated `CHANGELOG.md` per chart, following [Keep a Changelog](https://keepachangelog.com) format (`## [0.4.1] - 2026-06-28`, `### Added / Changed / Fixed / Removed` sections). This is the de-facto industry standard for Helm charts and is recognised by ArtifactHub. The root README retains only installation and repo-level docs. Migrate existing Arranger upgrade notes first as the reference implementation, then apply the pattern to all charts as they are touched. Tooling option: `git-chglog` or `conventional-commits` can automate CHANGELOG generation from commit messages if the team adopts conventional commits.

## Done

