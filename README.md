# Helm Chart for Harbor

**Notes:** The master branch is in heavy development, please use the codes on other branch instead.

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [Harbor](https://github.com/goharbor/harbor) in a Kubernetes cluster. Welcome to [contribute](CONTRIBUTING.md) to Helm Chart for Harbor.

## Prerequisites

- Kubernetes cluster 1.10+ with Beta APIs enabled
- Kubernetes Ingress Controller is enabled
- Helm 2.8.0+

## Installing the Chart

Download Harbor helm chart code.
```bash
git clone https://github.com/goharbor/harbor-helm
```
Checkout the branch.
```bash
cd harbor-helm
git checkout branch_name
```
Download external dependent charts required by Harbor chart.
```bash
helm dependency update
```
Install the Harbor helm chart with a release name `my-release`:
```bash
helm install --name my-release .
```

The command deploys Harbor on the Kubernetes cluster with the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured in values.yaml or via `--set` flag during installation.

**Notes:** If you want to disble the persistence by `--set`, two items need to be configured as `false`: `persistence.enabled` and `redis.master.persistence.enabled`.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Harbor chart and the default values.

| Parameter                  | Description                        | Default                 |
| -----------------------    | ---------------------------------- | ----------------------- |
| **Harbor** |
| `persistence.enabled`     | Persistent data | `true` |
| `externalURL`       | Ther external URL for Harbor core service | `https://core.harbor.domain` |
| `harborAdminPassword`  | The password of system admin | `Harbor12345` |
| `secretkey` | The key used for encryption. Must be a string of 16 chars | `not-a-secure-key` |
| `imagePullPolicy` | The image pull policy | `IfNotPresent` |
| **Ingress** |
| `ingress.enabled` | Enable ingress objects | `true` |
| `ingress.hosts.core` | The host of Harbor core service in ingress rule | `core.harbor.domain` |
| `ingress.hosts.notary` | The host of Harbor notary service in ingress rule | `notary.harbor.domain` |
| `ingress.annotations` | The annotations used in ingress | `true` |
| `ingress.tls.enabled` | Enable TLS | `true` |
| `ingress.tls.secretName` | Fill the secretName if you want to use the certificate of yourself when Harbor serves with HTTPS. A certificate will be generated automatically by the chart if leave it empty | |
| **Portal** |
| `portal.image.repository` | Repository for portal image | `goharbor/harbor-portal` |
| `portal.image.tag` | Tag for portal image | `dev` |
| `portal.replicas` | The replica count | `1` |
| `portal.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `portal.nodeSelector` | Node labels for pod assignment | `{}` |
| `portal.tolerations` | Tolerations for pod assignment | `[]` |
| `portal.affinity` | Node/Pod affinities | `{}` |
| **Adminserver** |
| `adminserver.image.repository` | Repository for adminserver image | `goharbor/harbor-adminserver` |
| `adminserver.image.tag` | Tag for adminserver image | `dev` |
| `adminserver.replicas` | The replica count | `1` |
| `adminserver.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `adminserver.nodeSelector` | Node labels for pod assignment | `{}` |
| `adminserver.tolerations` | Tolerations for pod assignment | `[]` |
| `adminserver.affinity` | Node/Pod affinities | `{}` |
| **Jobservice** |
| `jobservice.image.repository` | Repository for jobservice image | `goharbor/harbor-jobservice` |
| `jobservice.image.tag` | Tag for jobservice image | `dev` |
| `jobservice.replicas` | The replica count | `1` |
| `jobservice.volumes` | The volume used to store data if persistence is enabled | |
| `jobservice.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `jobservice.nodeSelector` | Node labels for pod assignment | `{}` |
| `jobservice.tolerations` | Tolerations for pod assignment | `[]` |
| `jobservice.affinity` | Node/Pod affinities | `{}` |
| **Core** |
| `core.image.repository` | Repository for Harbor core image | `goharbor/harbor-core` |
| `core.image.tag` | Tag for Harbor core image | `dev` |
| `core.replicas` | The replica count | `1` |
| `core.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `core.nodeSelector` | Node labels for pod assignment | `{}` |
| `core.tolerations` | Tolerations for pod assignment | `[]` |
| `core.affinity` | Node/Pod affinities | `{}` |
| **Database** |
| `database.type` | If external database is used, set it to `external` | `internal` |
| `database.internal.image.repository` | Repository for database image | `goharbor/harbor-db` |
| `database.internal.image.tag` | Tag for database image | `dev` |
| `database.internal.password` | The password for database | `changeit` |
| `database.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `database.internal.volumes` | The volume used to persistent data |
| `database.internal.nodeSelector` | Node labels for pod assignment | `{}` |
| `database.internal.tolerations` | Tolerations for pod assignment | `[]` |
| `database.internal.affinity` | Node/Pod affinities | `{}` |
| `database.external.host` | The hostname of external database | `192.168.0.1` |
| `database.external.port` | The port of external database | `5432` |
| `database.external.username` | The username of external database | `user` |
| `database.external.password` | The password of external database | `password` |
| `database.external.coreDatabase` | The database used by core service | `registry` |
| `database.external.sslmode` | Connection method of external database (require|prefer|disable) | `disable`|
| `database.external.clairDatabase` | The database used by clair | `clair` |
| `database.external.notaryServerDatabase` | The database used by Notary server | `notary_server` |
| `database.external.notarySignerDatabase` | The database used by Notary signer | `notary_signer` |
| **Registry** |
| `registry.registry.image.repository` | Repository for registry image | `goharbor/registry-photon` |
| `registry.registry.image.tag` | Tag for registry image | `dev` |
| `registry.registry.logLevel` | The log level | `info` |
| `registry.controller.image.repository` | Repository for registry controller image | `goharbor/harbor-registryctl` |
| `registry.controller.image.tag` | Tag for registry controller image | `dev` |
| `registry.replicas` | The replica count | `1` |
| `registry.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `registry.volumes` | used to create PVCs if persistence is enabled (see instructions in values.yaml) | see values.yaml |
| `registry.nodeSelector` | Node labels for pod assignment | `{}` |
| `registry.tolerations` | Tolerations for pod assignment | `[]` |
| `registry.affinity` | Node/Pod affinities | `{}` |
| **Chartmuseum** |
| `chartmuseum.enabled` | Enable chartmusuem to store chart | `true` |
| `chartmuseum.image.repository` | Repository for chartmuseum image | `goharbor/chartmuseum-photon` |
| `chartmuseum.image.tag` | Tag for chartmuseum image | `dev` |
| `chartmuseum.replicas` | The replica count | `1` |
| `chartmuseum.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined |
| `chartmuseum.volumes` | used to create PVCs if persistence is enabled (see instructions in values.yaml) | see values.yaml |
| `chartmuseum.nodeSelector` | Node labels for pod assignment | `{}` |
| `chartmuseum.tolerations` | Tolerations for pod assignment | `[]` |
| `chartmuseum.affinity` | Node/Pod affinities | `{}` |
| **Storage For Registry And Chartmuseum** |
| `storage.type` | The storage backend used for registry and chartmuseum: `filesystem`, `azure`, `gcs`, `s3`, `swift`, `oss` | `filesystem` |
| `other values` | The other values please refer to https://github.com/docker/distribution/blob/master/docs/configuration.md#storage |  |
| **Clair** |
| `clair.enabled` | Enable Clair? | `true` |
| `clair.image.repository` | Repository for clair image | `goharbor/clair-photon` |
| `clair.image.tag` | Tag for clair image | `dev`
| `clair.replicas` | The replica count | `1` |
| `clair.resources` | [resources](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) to allocate for container   | undefined
| `clair.nodeSelector` | Node labels for pod assignment | `{}` |
| `clair.tolerations` | Tolerations for pod assignment | `[]` |
| `clair.affinity` | Node/Pod affinities | `{}` |
| **Redis** |
| `redis.usePassword` | Whether use password | `false` |
| `redis.password` | The password for Redis | `changeit` |
| `redis.cluster.enabled` | Enable Redis cluster | `false` |
| `redis.master.persistence.enabled` | Persistent data   | `true` |
| `redis.external.enabled` | If an external Redis is used, set it to `true` | `false` |
| `redis.external.host` | The hostname of external Redis | `192.168.0.2` |
| `redis.external.port` | The port of external Redis | `6379` |
| `redis.external.databaseIndex` | The database index of external Redis | `0` |
| `redis.external.usePassword` | Whether use password for external Redis | `false` |
| `redis.external.password` | The password of external Redis | `changeit` |
| **Notary** |
| `notary.enabled` | Enable Notary? | `true` |
| `notary.server.image.repository` | Repository for notary server image | `goharbor/notary-server-photon` |
| `notary.server.image.tag` | Tag for notary server image | `dev`
| `notary.server.replicas` | The replica count | `1` |
| `notary.signer.image.repository` | Repository for notary signer image | `goharbor/notary-signer-photon` |
| `notary.signer.image.tag` | Tag for notary signer image | `dev`
| `notary.signer.replicas` | The replica count | `1` |
| `notary.nodeSelector` | Node labels for pod assignment | `{}` |
| `notary.tolerations` | Tolerations for pod assignment | `[]` |
| `notary.affinity` | Node/Pod affinities | `{}` |

## Persistence

By default, the Harbor persistents the data in a few of persistent volumes. The volumes are created using dynamic volume provisioning. If you want to use the existing persistent volume claims, specify them during installation.

When running a cluster without persistence, set the  `persistence.enabled` as `false`. Data does not survive the termination of a pod.
