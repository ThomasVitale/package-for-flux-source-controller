# FluxCD Source Controller

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [FluxCD Source Controller](https://fluxcd.io/docs/components/source), a source management component to provide a common interface for artifacts acquisition.

## Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
  ```

## Installation

First, add the [Kadras package repository](https://github.com/arktonix/kadras-packages) to your Kubernetes cluster.

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-repo \
    --url ghcr.io/arktonix/kadras-packages \
    -n kadras-packages
  ```

Then, install the FluxCD Source Controller package.

  ```shell
  kctrl package install -i fluxcd-source-controller \
    -p fluxcd-source-controller.packages.kadras.io \
    -v 0.32.1+kadras.1 \
    -n kadras-packages
  ```

### Verification

You can verify the list of installed Carvel packages and their status.

  ```shell
  kctrl package installed list -n kadras-packages
  ```

### Version

You can get the list of FluxCD Source Controller versions available in the Kadras package repository.

  ```shell
  kctrl package available list -p fluxcd-source-controller.packages.kadras.io -n kadras-packages
  ```

## Configuration

The FluxCD Source Controller package has the following configurable properties.

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespace` | `source-system` | The namespace where to install FluxCD Source Controller. |
| `resources.limits.cpu` | `1000m` | CPU limits for the `source-controller` Deployment. |
| `resources.limits.memory` | `1Gi` | Memory limits for the `source-controller` Deployment. |
| `service_port` | `80` | Port configuration for the `source-controller` Service. |
| `proxy.no_proxy` | `""` | For which domains the proxy should not be used. Used only if the `proxy.https_proxy` and `proxy.http_proxy` properties are not empty |
| `proxy.https_proxy` | `""` | The HTTPS proxy URL. |
| `proxy.http_proxy` | `""` | The HTTP proxy URL. |

You can define your configuration in a `values.yml` file.

  ```yaml
  namespace: source-system

  resources:
    limits:
      cpu: 1000m
    limits:
      memory: 1Gi

  service_port: 80

  proxy:
    no_proxy: ""
    https_proxy: ""
    http_proxy: ""
  ```

Then, reference it from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i fluxcd-source-controller \
    -p fluxcd-source-controller.packages.kadras.io \
    -v 0.32.1+kadras.1 \
    -n kadras-packages \
    --values-file values.yml
  ```

## Upgrading

You can upgrade an existing package to a newer version using `kctrl`.

  ```shell
  kctrl package installed update -i fluxcd-source-controller \
    -v <new-version> \
    -n kadras-packages
  ```

You can also update an existing package with a newer `values.yml` file.

  ```shell
  kctrl package installed update -i fluxcd-source-controller \
    -n kadras-packages \
    --values-file values.yml
  ```

## Other

The recommended way of installing the FluxCD Source Controller package is via the [Kadras package repository](https://github.com/arktonix/kadras-packages). If you prefer not using the repository, you can install the package by creating the necessary Carvel `PackageMetadata` and `Package` resources directly using [`kapp`](https://carvel.dev/kapp/docs/latest/install) or `kubectl`.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a fluxcd-source-controller-package -n kadras-packages -y \
    -f https://github.com/arktonix/package-for-fluxcd-source-controller/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/package-for-fluxcd-source-controller/releases/latest/download/package.yml
  ```

## Support and Documentation

For support and documentation specific to FluxCD Source Controller, check out [https://fluxcd.io/docs/components/source](https://fluxcd.io/docs/components/source).

## References

This package is based on the original FluxCD Source Controller package used in [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) before its retirement.

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
