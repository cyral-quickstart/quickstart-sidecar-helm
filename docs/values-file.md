# Values file configuration reference

This page describes the options that can be set in the `values.yaml` file to
configure your Helm sidecar.

**NOTE:** The default values also define the type of structure that should be
put in the field:

| Default Value | Variable Type  |
| ------------- | -------------- |
| []            | Array          |
| ""            | String         |
| {}            | Complex object |

## Kubernetes configuration

### Image configuration

| Field                  | Description                                     | Default Value |
| ---------------------- | ----------------------------------------------- | ------------- |
| `global.imageRegistry` | Overrides the image registry for sidecar images | ""            |
| `image.username`       | Username to access image registry               | ""            |
| `image.password_b64`   | B64 password to access container registry       | ""            |

### Object generation configuration

| Field              | Description                                           | Default Value |
| ------------------ | ----------------------------------------------------- | ------------- |
| `nameOverride`     | Overrides the name of the chart for object generation | ""            |
| `fullnameOverride` | Overrides the full name of the generated objects      | ""            |

### Cyral Control Plane configurations

| Field                     | Description                                                 | Default Value |
| ------------------------- | ----------------------------------------------------------- | ------------- |
| `controlPlane.host`       | Host of the Cyral Control Plane                             | ""            |
| `controlPlane.ports.http` | HTTP port for your control plane                            | 443           |
| `controlPlane.ports.grpc` | GRPC port for your control plane                            | 443           |
| `sidecarId`               | ID of the sidecar, as registered in the Cyral control plane | ""            |

### Forward Proxy configuration

| Field                        | Description                                   | Default Value |
| ---------------------------- | --------------------------------------------- | ------------- |
| `forwardProxy.clientId`      | Cyral Client ID                               | ""            |
| `forwardProxy.clientSecret`  | Cyral Client Secret                           | ""            |
| `forwardProxy.secretName`    | Name of the secret in Kubernetes (lower case) | ""            |
| `forwardProxy.secretKeyName` | Key of the secret in Kubernetes               | ""            |

### Deployment configuration

| Field                       | Description                                                                                                                                                                                                                       | Default Value |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `affinity`                  | Affinity configuration for the pods                                                                                                                                                                                               | {}            |
| `extraVolumes`              | Extra volumes to be created on the deployment                                                                                                                                                                                     | []            |
| `podAffinityPreset`         | Pod affinity preset. Ignored if `affinity` is set. Can be one of `hard` or `soft`. Creates an affinity between the deployment's pods. If set to hard, affinity is required, if set to soft, affinity is preferred.                | ""            |
| `podAntiAffinityPreset`     | Pod anti-affinity preset. Ignored if `affinity` is set. Can be one of `hard` or `soft`. Creates an anti-affinity clause between the deploment's pod. If set to hard, affinity is required, if set to soft, affinity is preferred. |               |
|                             | "soft"                                                                                                                                                                                                                            |               |
| `podAnnotations`            | Add custom annotations to the pod                                                                                                                                                                                                 | []            |
| `nodeAffinityPreset.type`   | Node affinity preset type. Ignored if `affinity` is set. Can be one of `hard` or `soft`. If set to hard, affinity is required, if set to soft, affinity is preferred.                                                             | ""            |
| `nodeAffinityPreset.key`    | Node label key to match for affinity definition. Ignored if `affinity` is set.                                                                                                                                                    | ""            |
| `nodeAffinityPreset.values` | Node label values to match for affinity definition. Ignored if `affinity` is set.                                                                                                                                                 | []            |
| `nodeSelector`              | Node selector for the pods of the deployment                                                                                                                                                                                      | {}            |
| `replicaCount`              | Number of replicas of the deployment                                                                                                                                                                                              | 1             |
| `podSecurityContext`        | Security context for each pod created in the deployment                                                                                                                                                                           | {}            |
| `securityContext`           | Security context for each container created in the deployment                                                                                                                                                                     | {}            |
| `tolerations`               | Tolerations for the pods of the deployment                                                                                                                                                                                        | []            |

### Service configuration

| Field                                | Description                                                                                                                     | Default Value |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `service.enable`                     | Enables/disables creation of the sidecar `service`                                                                              | `true`        |
| `service.type`                       | Type of kubernetes service that will be created alongside the sidecar. Allowed values are ClusterIP, LoadBalancer and NodePort. | "ClusterIP"   |
| `service.loadBalancer`               | Configuration for the AWS LB created when `service.type` is "LoadBalancer"                                                      | -             |
| `service.loadBalancer.certificateId` | Id for the certificate that the AWS LB will use                                                                                 | ""            |
| `service.loadBalancer.dnsName`       | DNS name for the AWS LB                                                                                                         | ""            |
| `service.loadBalancer.sourceRanges`  | Source ranges for reaching the AWS LB                                                                                           | []            |
| `service.loadBalancer.tlsPorts`      | Ports that will be TLS terminated in the load balancer                                                                          | []            |
| `service.ports`                      | Ports to be exposed on the service. These ports will override and other port defined in specific services                       | []            |

### TLS certificate configuration

| Field                                  | Description                                     | Default Value |
| -------------------------------------- | ----------------------------------------------- | ------------- |
| `dispatcher.pullCertificatesFromVault` | Whether or not to pull certificates from vault. | `false`       |

### RBAC configuration

| Field                        | Description                                              | Default Value |
| ---------------------------- | -------------------------------------------------------- | ------------- |
| `rbac.create`                | Whether or not to create RBAC objects on the cluster     | `true`        |
| `rbac.serviceAccount.name`   | Name of the service account that will be used by the pod | ""            |
| `rbac.serviceAccount.create` | Whether or not to create RBAC service account            | `true`        |

## Container configuration

These configurations can be applied to all containers inside of the chart, both
repository and infrastructure containers. To apply these on the `values.yaml`
file, you add the configuration to its `<container>` section.

### General configuration

| Field                           | Description                                                                                     | Default Value |
| ------------------------------- | ----------------------------------------------------------------------------------------------- | ------------- |
| `<container>.enabled`           | Whether or not to add the container to the sidecar                                              | `true`        |
| `<container>.extraEnvs`         | Extra environment variables that will be added to that container. These variables are templated | []            |
| `<container>.extraVolumeMounts` | Extra volumes to be mounted on that container. These values are templated                       | []            |

### Image configuration

| Field                          | Description                              | Default Value      |
| ------------------------------ | ---------------------------------------- | ------------------ |
| `<container>.image.registry`   | Image registry for the container image   | Container specific |
| `<container>.image.repository` | Image repository for the container image | Container specific |
| `<container>.image.tag`        | Image tag for the container image        | Container specific |

### Resource configuration

**IMPORTANT:** We don't set default resources for the deployment. This is
infrastructure specific.

**NOTE:** To read more about container resources on Kubernetes, read
[our guide on setting resources](./scenarios/resources.mdx) and
[Kubernetes' documentation on it](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

| Field                                   | Description                       | Default Value |
| --------------------------------------- | --------------------------------- | ------------- |
| `<container>.resources.limits.cpu`      | CPU limit for that container      | -             |
| `<container>.resources.limits.memory`   | memory limit for that container   | -             |
| `<container>.resources.requests.cpu`    | CPU request for that container    | -             |
| `<container>.resources.requests.memory` | memory request for that container | -             |

## Repository Configuration

This configuration applies for all repository specific containers. To apply this
to the values.yaml file, add it in the section for the `<repo>` type you want to
apply it to. For example, you would set the ports configuration for MySQL
databases in the parameter, `mysql.ports`. Most repositories also contain all
configurations contained in the
[Container Configuration](#container-configuration) section.

### General configuration

| Field                  | Description                                                                                                                                            | Default Value |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `<repo>.enabled`       | Whether the repository will be supported by the sidecar. If disabled the containers (if any) and ports will not be added to the final sidecar resource | `true`        |
| `<repo>.ports`         | Port configuration for the repository                                                                                                                  | -             |
| `<repo>.ports.sidecar` | List of ports that will be exposed on the sidecar's service with this repo's id on it. These ports will be overriten if `service.ports` is set         | Repo specific |

### Snowflake

| Field                       | Description                               | Default Value |
| --------------------------- | ----------------------------------------- | ------------- |
| `snowflake.idp.SSOLoginURL` | URL for the SSO for snowflake connections | ""            |
| `snowflake.idp.certificate` | Certificate for the snowflake SSO         | ""            |

**NOTE:** Denodo and redshift repositories do not have image nor resource
configurations, since they are handled by the same container as the `postgres`
repository.

## Integration configuration

These configurations will usually be generated by your control plane, **don't
change these** without help from Cyral support.

### HashiCorp Vault

| Field                            | Description                                                      | Default Value |
| -------------------------------- | ---------------------------------------------------------------- | ------------- |
| `vaultIntegration.integrationId` | ID of a HashiCorp Vault integration defined in the Control Plane | ""            |
