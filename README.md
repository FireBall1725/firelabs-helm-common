# FireLabs Helm Common Library

![Version: 5.0.0](https://img.shields.io/badge/Version-5.0.0-informational?style=flat-square) ![Type: library](https://img.shields.io/badge/Type-library-informational?style=flat-square)

**Function library for Helm charts**

This library reduces maintenance cost and promotes DRY (Don't Repeat Yourself) principles for Helm charts that follow similar patterns.

> **Note:** This library is forked from [k8s-at-home/library-charts](https://github.com/k8s-at-home/library-charts) and continues active development with modern features and enhancements.

## Requirements

Kubernetes: `>=1.16.0-0`

## Dependencies

| Repository | Name | Version |
|------------|------|---------|

## Installing the Chart

This is a [Helm Library Chart](https://helm.sh/docs/topics/library_charts/#helm).

**WARNING: THIS CHART IS NOT MEANT TO BE INSTALLED DIRECTLY**

## Using this library

Include this chart as a dependency in your `Chart.yaml`:

```yaml
# Chart.yaml
dependencies:
- name: common
  version: 5.0.0
  repository: https://fireball1725.github.io/firelabs-helm-common/
```

## Configuration

Read through the [values.yaml](./values.yaml) file. It has several commented out suggested values.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| additionalContainers | object | `{}` | Specify any additional containers here as dictionary items. Each additional container should have its own key. Helm templates can be used. |
| addons | object | See below | The common chart supports several add-ons. These can be configured under this key. |
| addons.codeserver | object | See values.yaml | The common library supports adding a code-server add-on to access files. |
| addons.codeserver.args | list | `["--auth","none"]` | Set codeserver command line arguments. Consider setting --user-data-dir to a persistent location to preserve code-server setting changes |
| addons.codeserver.enabled | bool | `false` | Enable running a code-server container in the pod |
| addons.codeserver.env | object | `{}` | Set any environment variables for code-server here |
| addons.codeserver.git | object | See below | Optionally allow access a Git repository by passing in a private SSH key |
| addons.codeserver.git.deployKey | string | `""` | Raw SSH private key |
| addons.codeserver.git.deployKeyBase64 | string | `""` | Base64-encoded SSH private key. When both variables are set, the raw SSH key takes precedence. |
| addons.codeserver.git.deployKeySecret | string | `""` | Existing secret containing SSH private key The chart expects it to be present under the `id_rsa` key. |
| addons.codeserver.image.pullPolicy | string | `"IfNotPresent"` | Specify the code-server image pull policy |
| addons.codeserver.image.repository | string | `"ghcr.io/coder/code-server"` | Specify the code-server image |
| addons.codeserver.image.tag | string | `"4.5.1"` | Specify the code-server image tag |
| addons.codeserver.ingress.enabled | bool | `false` | Enable an ingress for the code-server add-on. |
| addons.codeserver.ingress.ingressClassName | string | `nil` | Set the ingressClass that is used for this ingress. Requires Kubernetes >=1.19 |
| addons.codeserver.service.enabled | bool | `true` | Enable a service for the code-server add-on. |
| addons.codeserver.volumeMounts | list | `[]` | Specify a list of volumes that get mounted in the code-server container. At least 1 volumeMount is required! |
| addons.codeserver.workingDir | string | `""` | Specify the working dir that will be opened when code-server starts If not given, the app will default to the mountpah of the first specified volumeMount |
| addons.netshoot | object | See values.yaml | The common library supports adding a netshoot add-on to troubleshoot network issues within a Pod. |
| addons.netshoot.enabled | bool | `false` | Enable running a netshoot container in the pod |
| addons.netshoot.env | object | `{}` | Set any environment variables for netshoot here |
| addons.netshoot.image.pullPolicy | string | `"IfNotPresent"` | Specify the netshoot image pull policy |
| addons.netshoot.image.repository | string | `"ghcr.io/nicolaka/netshoot"` | Specify the netshoot image |
| addons.netshoot.image.tag | string | `"v0.7"` | Specify the netshoot image tag |
| addons.promtail | object | See values.yaml | The common library supports adding a promtail add-on to to access logs and ship them to loki. |
| addons.promtail.args | list | `[]` | Set promtail command line arguments |
| addons.promtail.enabled | bool | `false` | Enable running a promtail container in the pod |
| addons.promtail.env | object | `{}` | Set any environment variables for promtail here |
| addons.promtail.image.pullPolicy | string | `"IfNotPresent"` | Specify the promtail image pull policy |
| addons.promtail.image.repository | string | `"docker.io/grafana/promtail"` | Specify the promtail image |
| addons.promtail.image.tag | string | `"2.6.1"` | Specify the promtail image tag |
| addons.promtail.logs | list | `[]` | The paths to logs on the volume |
| addons.promtail.loki | string | `""` | The URL to Loki |
| addons.promtail.volumeMounts | list | `[]` | Specify a list of volumes that get mounted in the promtail container. At least 1 volumeMount is required! |
| addons.vpn | object | See values.yaml | The common chart supports adding a VPN add-on. |
| addons.vpn.args | list | `[]` | Override the args for the vpn sidecar container |
| addons.vpn.configFile | string | `nil` | Provide a customized vpn configuration file to be used by the VPN. |
| addons.vpn.configFileSecret | string | `nil` | Reference an existing secret that contains the VPN configuration file The chart expects it to be present under the `vpnConfigfile` key. |
| addons.vpn.enabled | bool | `false` | Enable running a VPN in the pod to route traffic through a VPN |
| addons.vpn.env | object | `{}` | All variables specified here will be added to the vpn sidecar container See the documentation of the VPN image for all config values |
| addons.vpn.gluetun | object | See below | Gluetun specific configuration -- Make sure to read the [documentation](https://github.com/qdm12/gluetun/wiki) |
| addons.vpn.gluetun.image.pullPolicy | string | `"IfNotPresent"` | Specify the Gluetun image pull policy |
| addons.vpn.gluetun.image.repository | string | `"docker.io/qmcgaw/gluetun"` | Specify the Gluetun image |
| addons.vpn.gluetun.image.tag | string | `"v3.30.0"` | Specify the Gluetun image tag |
| addons.vpn.livenessProbe | object | `{}` | Optionally specify a livenessProbe |
| addons.vpn.networkPolicy.annotations | object | `{}` | Provide additional annotations which may be required. |
| addons.vpn.networkPolicy.egress | string | `nil` | The egress configuration for your network policy |
| addons.vpn.networkPolicy.enabled | bool | `false` | If set to true, will deploy a network policy that blocks all outbound traffic except traffic specified as allowed |
| addons.vpn.networkPolicy.labels | object | `{}` | Provide additional labels which may be required. |
| addons.vpn.networkPolicy.podSelectorLabels | object | `{}` | Provide additional podSelector labels which may be required. |
| addons.vpn.openvpn | object | See below | OpenVPN specific configuration |
| addons.vpn.openvpn.auth | string | `nil` | Credentials to connect to the VPN Service |
| addons.vpn.openvpn.authSecret | string | `nil` | Optionally specify an existing secret that contains the credentials |
| addons.vpn.openvpn.image.pullPolicy | string | `"IfNotPresent"` | Specify the openvpn client image pull policy |
| addons.vpn.openvpn.image.repository | string | `"dperson/openvpn-client"` | Specify the openvpn client image |
| addons.vpn.openvpn.image.tag | string | `"latest"` | Specify the openvpn client image tag |
| addons.vpn.scripts | object | See values.yaml | Provide custom up/down scripts that can be used by the vpn configuration. |
| addons.vpn.securityContext | object | See values.yaml | Set the VPN container securityContext |
| addons.vpn.type | string | `"openvpn"` | Specify the VPN type. Valid options are `openvpn`, `wireguard` and `gluetun`. |
| addons.vpn.wireguard | object | See below | WireGuard specific configuration |
| addons.vpn.wireguard.image.pullPolicy | string | `"IfNotPresent"` | Specify the WireGuard image pull policy |
| addons.vpn.wireguard.image.repository | string | `"ghcr.io/k8s-at-home/wireguard"` | Specify the WireGuard image |
| addons.vpn.wireguard.image.tag | string | `"v1.0.20210914"` | Specify the WireGuard image tag |
| affinity | object | `{}` | Defines affinity constraint rules. |
| args | list | `[]` | Override the args for the default container |
| automountServiceAccountToken | bool | `true` | Specifies whether a service account token should be automatically mounted. |
| autoscaling | object | <disabled> | Add a Horizontal Pod Autoscaler |
| command | list | `[]` | Override the command(s) for the default container |
| configmap | object | See below | Configure configMaps for the chart here. |
| controller.enabled | bool | `true` | enable the controller. |
| controller.type | string | `"deployment"` | Set the controller type. Valid options are deployment, daemonset or statefulset |
| controller.replicas | int | `1` | Number of desired pods |
| env | string | `nil` | Main environment variables. Template enabled. |
| image.repository | string | `nil` | image repository |
| image.tag | string | `nil` | image tag |
| image.pullPolicy | string | `nil` | image pull policy |
| ingress | object | See below | Configure the ingresses for the chart here. |
| persistence | object | See below | Configure persistence for the chart here. |
| service | object | See below | Configure the services for the chart here. |
| serviceAccount.create | bool | `false` | Specifies whether a service account should be created |

For the complete values documentation, see [values.yaml](./values.yaml).

## Changelog

All notable changes to this library Helm chart will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

### [5.0.0]

#### Changed

- **BREAKING:** Rebranded from k8s-at-home to FireLabs Helm Common Library
- Updated repository URLs and maintainer information
- Removed deprecation warnings

#### Migration Notes

If you're migrating from k8s-at-home common library (v4.x), update your Chart.yaml:

```yaml
dependencies:
- name: common
  version: 5.0.0
  repository: https://fireball1725.github.io/firelabs-helm-common/
```

For previous changelog entries from the k8s-at-home fork, see [CHANGELOG_LEGACY.md](./CHANGELOG_LEGACY.md).

## Support

- Repository: [GitHub](https://github.com/FireBall1725/firelabs-helm-common)
- Issues: [GitHub Issues](https://github.com/FireBall1725/firelabs-helm-common/issues)

## Credits

This project is a fork of the excellent [k8s-at-home/library-charts](https://github.com/k8s-at-home/library-charts) project.

---

Autogenerated from chart metadata using [helm-docs](https://github.com/norwoodj/helm-docs)
