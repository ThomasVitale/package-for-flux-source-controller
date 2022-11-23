# FluxCD Source Controller

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [FluxCD Source Controller](https://fluxcd.io/docs/components/source/), a source management component to provide a common interface for artifacts acquisition.

## Components

* FluxCD Source Controller

## Prerequisites

* Install the [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI to manage Carvel packages in a convenient way.
* Ensure [kapp-controller](https://carvel.dev/kapp-controller) is deployed in your Kubernetes cluster. You can do that with Carvel
[`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

```shell
kapp deploy -a kapp-controller -y \
  -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
```

## Installation

You can install the FluxCD Source Controller package directly or rely on the [Kadras package repository](https://github.com/arktonix/carvel-packages)
(recommended choice).

Follow the [instructions](https://github.com/arktonix/carvel-packages) to add the Kadras package repository to your Kubernetes cluster.

If you don't want to use the Kadras package repository, you can create the necessary `PackageMetadata` and
`Package` resources for the FluxCD Source Controller package directly.

```shell
kubectl create namespace carvel-packages
kapp deploy -a fluxcd-source-controller-package -n carvel-packages -y \
    -f https://github.com/arktonix/package-for-fluxcd-source-controller/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/package-for-fluxcd-source-controller/releases/latest/download/package.yml
```

Either way, you can then install the FluxCD Source Controller package using [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl).

```shell
kctrl package install -i fluxcd-source-controller \
    -p fluxcd-source-controller.packages.kadras.io \
    -v 0.32.1 \
    -n carvel-packages
```

You can retrieve the list of available versions with the following command.

```shell
kctrl package available list -p fluxcd-source-controller.packages.kadras.io
```

You can check the list of installed packages and their status as follows.

```shell
kctrl package installed list -n carvel-packages
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
    -v 0.32.1 \
    -n carvel-packages \
    --values-file values.yml
```

## Documentation

For documentation specific to FluxCD Source Controller, check out [https://fluxcd.io/docs/components/source](https://fluxcd.io/docs/components/source).

## References

This package is based on the original FluxCD Source Controller package used in [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) before its retirement.

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
