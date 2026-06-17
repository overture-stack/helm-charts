# helm-charts — repo-level tech debt

## Jenkinsfile pipeline library branch reference

`Jenkinsfile` currently pins `jenkins-pipeline-library@add-softeng-charts` because that is the branch where `pipelineSoftEngBuildHelmCharts` was fixed to respect `config.credentialsId` and `config.ociBase`. Once that branch merges to `main` in the pipeline library, update the Jenkinsfile to `@main` (or the settled stable ref).

**Standalone:** yes.
**Context:** OCI publishing setup added 2026-06-17; `add-softeng-charts` is a temporary branch ref.
