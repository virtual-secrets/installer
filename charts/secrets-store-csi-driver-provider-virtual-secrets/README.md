# Virtual Secrets Provider for Secret Store CSI Driver

[Virtual Secrets Provider for Secret Store CSI Driver by AppsCode](https://github.com/virtual-secrets) - Virtual Secrets Provider for Secret Store CSI Driver by AppsCode

## TL;DR;

```bash
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update
$ helm search repo appscode/secrets-store-csi-driver-provider-virtual-secrets --version=v2025.3.14
$ helm upgrade -i secrets-store-csi-driver-provider-virtual-secrets appscode/secrets-store-csi-driver-provider-virtual-secrets -n kube-system --create-namespace --version=v2025.3.14
```

## Introduction

This chart deploys a Virtual Secrets Provider for Secret Store CSI Driver on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.26+

## Installing the Chart

To install/upgrade the chart with the release name `secrets-store-csi-driver-provider-virtual-secrets`:

```bash
$ helm upgrade -i secrets-store-csi-driver-provider-virtual-secrets appscode/secrets-store-csi-driver-provider-virtual-secrets -n kube-system --create-namespace --version=v2025.3.14
```

The command deploys a Virtual Secrets Provider for Secret Store CSI Driver on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall the `secrets-store-csi-driver-provider-virtual-secrets`:

```bash
$ helm uninstall secrets-store-csi-driver-provider-virtual-secrets -n kube-system
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the `secrets-store-csi-driver-provider-virtual-secrets` chart and their default values.

|                Parameter                 |              Description              |                            Default                             |
|------------------------------------------|---------------------------------------|----------------------------------------------------------------|
| registryFQDN                             |                                       | <code>ghcr.io</code>                                           |
| image.registry                           |                                       | <code>appscode</code>                                          |
| image.repository                         |                                       | <code>secrets-store-csi-driver-provider-virtual-secrets</code> |
| image.pullPolicy                         |                                       | <code>IfNotPresent</code>                                      |
| image.tag                                |                                       | <code>v0.0.1</code>                                            |
| nameOverride                             |                                       | <code>""</code>                                                |
| fullnameOverride                         |                                       | <code>""</code>                                                |
| providerVolume                           |                                       | <code>"/etc/kubernetes/secrets-store-csi-providers"</code>     |
| kubeletPath                              |                                       | <code>"/var/lib/kubelet"</code>                                |
| podLabels                                |                                       | <code>{}</code>                                                |
| podAnnotations                           |                                       | <code>{}</code>                                                |
| affinity                                 |                                       | <code>{}</code>                                                |
| resources.requests.cpu                   |                                       | <code>50m</code>                                               |
| resources.requests.memory                |                                       | <code>100Mi</code>                                             |
| resources.limits.cpu                     |                                       | <code>50m</code>                                               |
| resources.limits.memory                  |                                       | <code>100Mi</code>                                             |
| priorityClassName                        |                                       | <code>""</code>                                                |
| nodeSelector                             |                                       | <code>{}</code>                                                |
| tolerations                              |                                       | <code>[]</code>                                                |
| port                                     |                                       | <code>8989</code>                                              |
| updateStrategy.type                      |                                       | <code>RollingUpdate</code>                                     |
| imagePullSecrets                         |                                       | <code>[]</code>                                                |
| rbac.install                             |                                       | <code>true</code>                                              |
| securityContext.privileged               |                                       | <code>false</code>                                             |
| securityContext.allowPrivilegeEscalation |                                       | <code>false</code>                                             |
| distro.openshift                         | Set true, if installed in OpenShift   | <code>false</code>                                             |
| distro.ubi                               | Set operator or all to use ubi images | <code>""</code>                                                |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm upgrade -i`. For example:

```bash
$ helm upgrade -i secrets-store-csi-driver-provider-virtual-secrets appscode/secrets-store-csi-driver-provider-virtual-secrets -n kube-system --create-namespace --version=v2025.3.14 --set registryFQDN=ghcr.io
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while
installing the chart. For example:

```bash
$ helm upgrade -i secrets-store-csi-driver-provider-virtual-secrets appscode/secrets-store-csi-driver-provider-virtual-secrets -n kube-system --create-namespace --version=v2025.3.14 --values values.yaml
```
