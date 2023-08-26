# Flux Source Controller

![Test Workflow](https://github.com/kadras-io/package-for-fluxcd-source-controller/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/package-for-fluxcd-source-controller/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v1.0/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

A Carvel package for [Flux Source Controller](https://fluxcd.io/docs/components/source), a source management component to provide a common interface for artifacts acquisition.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.26+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages --create-namespace
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the Flux Source Controller package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a flux-source-controller-package -n kadras-packages -y \
    -f https://github.com/kadras-io/package-for-flux-source-controller/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/package-for-flux-source-controller/releases/latest/download/package.yml
  ```
</details>

Install the Flux Source Controller package:

  ```shell
  kctrl package install -i flux-source-controller \
    -p flux-source-controller.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p flux-source-controller.packages.kadras.io -n kadras-packages
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-packages
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to Flux Source Controller, check out [fluxcd.io/docs/components/source](https://fluxcd.io/docs/components/source).

## 🎯&nbsp; Configuration

The Flux Source Controller package can be customized via a `values.yml` file.

  ```yaml
  resources:
    limits:
      cpu: 1000m
    limits:
      memory: 1Gi
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i flux-source-controller \
    -p flux-source-controller.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages \
    --values-file values.yml
  ```

### Values

The kpack package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespace` | `flux-system` | The namespace where to install Flux Source Controller. |
| `policies.include` | `false` | Whether to include the out-of-the-box Kyverno policies to validate and secure the package installation. |
| `resources.limits.cpu` | `1000m` | CPU limits configuration for the `source-controller` Deployment. |
| `resources.limits.memory` | `1Gi` | Memory limits configuration for the `source-controller` Deployment. |
| `service_port` | `80` | Port configuration for the `source-controller` Service. |
| `proxy.http_proxy` | `""` | The HTTP proxy to use for network traffic. |
| `proxy.https_proxy` | `""` | The HTTPS proxy to use for network traffic. |
| `proxy.no_proxy` | `""` | A comma-separated list of hostnames, IP addresses, or IP ranges in CIDR format that should not use the proxy. |
| `logging.level` | `info` | Log verbosity level. Options: `trace`, `debug`, `info`, `error`. |
| `logging.encoding` | `json` | Log encoding format. Options: `console`, `json`. |
| `leader_election.lease_duration` | `35s` | Interval at which non-leader candidates will wait to force acquire leadership. |
| `leader_election.release_on_cancel` | `true` | Defines if the leader should step down voluntarily on controller manager shutdown. |
| `leader_election.renew_deadline` | `30s` | Duration that the leading controller manager will retry refreshing leadership before giving up. |
| `leader_election.retry_period` | `5s` | Duration the LeaderElector clients should wait between tries of actions. |

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.

## 🙏&nbsp; Acknowledgments

This package is inspired by the original source-controller package used in the [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) project before its retirement.
