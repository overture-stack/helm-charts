# EGO

https://ego.staging.overture.bio/

[EGO](https://github.com/overture-stack/ego) is a scalable stateless Authorization Service for Federated Identities including Google and Facebook

## Introduction

This chart bootstraps an EGO deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Persistence Volumes provision support in the underlying infrastructure
- Ingress that manages external access to the services in a cluster (if you want to enable ingress)

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install my-release overture/ego
```

The command deploys EGO on the Kubernetes cluster with the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

# Uninstalling the Chart

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and delete the release.

## Configuration

The following tables lists the configurable parameters of the EGO chart and their default values.

| Parameters                       | Description                                 | Default                                                                     |
| :------------------------------- | :------------------------------------------ | :-------------------------------------------------------------------------- |
| `replicaCount`                   | How many instances of EGO servers to be run | 1                                                                           |
| `image.repository`               | EGO image name                              | overture/ego                                                                |
| `image.tag`                      | EGO image tag                               | `latest`                                                                    |
| `uiImage.tag`                    | EGO ui image tag                            | `latest`                                                                    |
| `image.pullPolicy`               | EGO image pull policy                       | `IfNotPresent`                                                              |
| `ingress.enabled`                | EGO backend server port                     | `false`                                                                     |
| `ingress.host`                   | Ingress hosts                               | `ego.local`                                                                 |
| `appConfig.host`                 | Host name for EGO backend server            | `ego.local`                                                                 |
| `appConfig.linkedInClientID`     | LinkedIn Client ID                          | `nil`                                                                       |
| `appConfig.linkedInClientSecret` | LinkedIn Client Secret                      | `nil`                                                                       |
| `appConfig.githubClientID`       | GitHub Client ID                            | `nil`                                                                       |
| `appConfig.githubClientSecret`   | GitHub Client Secret                        | `nil`                                                                       |
| `postgres`                       | Configuration for PostgreSQL subchart       | `{postgresUsername: postgres, postgresPassword: password, postgresDb: ego}` |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set postgres.postgresqlPassword=secretpassword,postgres.postgresDb=my-database \
    overture/ego
```

The above command sets the PostgreSQL `postgres` account password to `secretpassword`. Additionally it creates a database named `my-database`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml overture/ego
```
