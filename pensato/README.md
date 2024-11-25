# Pensato Helm Chart

https://github.com/overture-stack/pensato

Pensato is an open-source SFTP and SCP server that provides access authorization using Ego API Keys.

## Introduction

This chart bootstraps an **Pensato** deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

> Note: Not all Pensato profiles are available in this chart as of `version 1.0.x` . It is pre-configured to use `ego,localdir` profiles, restricting it to use with Ego for authorization and a mounted volume for data storage.

## Prerequisites

- Persistence Volumes provision support in the underlying infrastructure
- Ingress that manages external access to the services in a cluster (if you want to enable ingress)

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release overture/pensato
```

The command deploys Pensato on the Kubernetes cluster with the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

# Uninstalling the Chart

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and delete the release.
