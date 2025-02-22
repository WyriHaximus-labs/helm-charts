# RabbitMQ

![Version: 0.4.3](https://img.shields.io/badge/Version-0.4.3-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 3.9.10](https://img.shields.io/badge/AppVersion-3.9.10-informational?style=flat-square)

A Helm chart for a RabbitMQ HA-cluster on Kubernetes

## TL;DR

```bash
$ helm repo add groundhog2k https://groundhog2k.github.io/helm-charts/
$ helm install my-release groundhog2k/rabbitmq
```

## Introduction

This chart uses the original [RabbitMQ image from Docker Hub](https://hub.docker.com/_/rabbitmq) to deploy a stateful RabbitMQ cluster in Kubernetes.

It fully supports deployment of the multi-architecture docker image.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.x
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release groundhog2k/rabbitmq
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm uninstall my-release
```

## Common parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| fullnameOverride | string | `""` | Fully override the deployment name |
| nameOverride | string | `""` | Partially override the deployment name |

## Deployment parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"rabbitmq"` | Image name |
| image.tag | string | `""` | Image tag |
| initImage.pullPolicy | string | `"IfNotPresent"` | Init image pull policy |
| initImage.repository | string | `"busybox"` | Init image name |
| initImage.tag | string | `"latest"` | Init image tag |
| imagePullSecrets | list | `[]` | Image pull secrets |
| livenessProbe | object | `see values.yaml` | Liveness probe configuration |
| readinessProbe | object | `see values.yaml` | Readiness probe configuration |
| customLivenessProbe | object | `{}` | Custom liveness probe (overwrites default liveness probe configuration) |
| customReadinessProbe | object | `{}` | Custom readiness probe (overwrites default readiness probe configuration) |
| resources | object | `{}` | Resource limits and requests |
| nodeSelector | object | `{}` | Deployment node selector |
| podAnnotations | object | `{}` | Additional pod annotations |
| podSecurityContext | object | `see values.yaml` | Pod security context |
| securityContext | object | `see values.yaml` | Container security context |
| env | list | `[]` | Additional container environmment variables |
| args | list | `[]` | Additional container command arguments |
| rbac.create | bool | `true` | Enable creation of RBAC |
| serviceAccount.annotations | object | `{}` | Additional service account annotations |
| serviceAccount.create | bool | `true` | Enable service account creation |
| serviceAccount.name | string | `""` | Optional name of the service account |
| affinity | object | `{}` | Affinity for pod assignment |
| tolerations | list | `[]` | Tolerations for pod assignment |
| podManagementPolicy | string | `"OrderedReady"` | Pod management policy |
| updateStrategyType | string | `"RollingUpdate"` | Pod update strategy |
| replicaCount | int | `1` | Number of replicas |
| revisionHistoryLimit | int | `nil` | Maximum number of revisions maintained in revision history
| podDisruptionBudget | object | `{}` | Pod disruption budget |
| podDisruptionBudget.minAvailable | int | `nil` | Minimum number of pods that must be available after eviction |
| podDisruptionBudget.maxUnavailable | int | `nil` | Maximum number of pods that can be unavailable after eviction |

## Service parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.type | string | `"ClusterIP"` | Service type |
| service.clusterIP | string | `nil` | The cluster ip address (only relevant for type LoadBalancer or NodePort) |
| service.loadBalancerIP | string | `nil` | The load balancer ip address (only relevant for type LoadBalancer) |
| service.amqp.port | int | `5672` | AMQP service port |
| service.amqp.nodePort | int | `nil` | Service node port (only relevant for type LoadBalancer or NodePort)|
| service.amqps.port | int | `5671` | Secure AMQP service port |
| service.amqps.nodePort | int | `nil` | Service node port (only relevant for type LoadBalancer or NodePort)|
| service.mgmt.port | int | `15672` | Management UI service port |
| service.mgmt.nodePort | int | `nil` | Service node port (only relevant for type LoadBalancer or NodePort) |
| service.prometheus.port | int | `15692` | Prometheus service port |
| service.prometheus.nodePort | int | `nil` | Service node port (only relevant for type LoadBalancer or NodePort) |

## Extra services parameters

Section to define custom services

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| extraServices[].name | string | `nil` | Unique name of the input service |
| extraServices[].type | string | `nil` | Service type (ClusterIP / NodePort / LoadBalancer) |
| extraServices[].protocol | string | `nil` | Protocol type (TCP / UDP) |
| extraServices[].containerPort | int | `nil` | Container port |
| extraServices[].port | int | `nil` | Service port |
| extraServices[].nodePort | int | `nil` | The node port (only relevant for type LoadBalancer or NodePort) |
| extraServices[].clusterIP | string | `nil` | The cluster ip address (only relevant for type LoadBalancer or NodePort) |
| extraServices[].loadBalancerIP | string | `nil` | The load balancer ip address (only relevant for type LoadBalancer) |

## Storage parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| storage.accessModes[0] | string | `"ReadWriteOnce"` | Storage access mode |
| storage.persistentVolumeClaimName | string | `nil` | PVC name when existing storage volume should be used |
| storage.requestedSize | string | `nil` | Size for new PVC, when no existing PVC is used |
| storage.className | string | `nil` | Storage class name |

## Ingress parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ingress.enabled | bool | `false` | Enable ingress for the Management UI service |
| ingress.annotations | string | `nil` | Additional annotations for ingress |
| ingress.hosts[0].host: | string | `""` | Hostname for the ingress endpoint |
| ingress.hosts[0].host.paths[0] | string | `"/"` | Path for the RabbitMQ Management UI |
| ingress.tls | list | `[]` | Ingress TLS parameters |

## RabbitMQ base parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| clusterDomain | string | `"cluster.local"` | Kubernetes cluster domain (DNS) suffix |
| plugins | list | `[]` | List of additional RabbitMQ plugins that should be activated (see: https://www.rabbitmq.com/plugins.html) |
| authentication.user | string | `"guest"` | Initial user name |
| authentication.password | string | `"guest"` | Initial password |
| authentication.erlangCookie | string | `nil` | Erlang cookie (default: Random base64 value) |
| clustering.rebalance | bool | `true` | Enable rebalance queues with master when new replica is created |
| clustering.useLongName | bool | `true` | Use FQDN for RabbitMQ node names |

## RabbitMQ memory parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| options.memoryHighWatermark.enabled | bool | `false` | Enables high memory watermark configuration |
| options.memoryHighWatermark.type | string | `"relative"` | Type of watermark value (relative or absolute) |
| options.memoryHighWatermark.value | float | `0.4` | Watermark value (default: 40%) |
| options.memoryHighWatermark.pagingRatio | float | `nil` | Paging threshold when RabbitMQ starts paging queue content before high memory watermark is reached |
| options.memory.totalAvailableOverrideValue | int | `nil` | Overwrites the value that is automatically calculated from resource.limits.memory |
| options.memory.calculationStrategy | string | `nil` | Strategy for memory usage report (rss or allocated) |

## RabbitMQ communication parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| options.tcp.port | int | `5672` | AMQP tcp port |
| options.ssl.enabled | bool | `false` | Enable secure AMQP (amqps) |
| options.ssl.port | int | `5671` | AMQPS tcp port |
| options.ssl.verify | bool | `false` | Enables or disables peer verification |
| options.ssl.failIfNoPeerCert | bool | `false` | Reject TLS connection when client fails to provide a certificate |
| options.ssl.depth | int | `nil` | Client certificate verification depth |

## RabbitMQ certificate parameters

Section for certificate support
(cacert,cert,key,password will be used for AMQP-over-SSL (AMPQS) - see: options.ssl)

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| certificates.enabled | bool | `false` | Enable mounting following certificates into folder /ssl |
| certificates.cacert | string | `nil` | CA certificate(s) in base64 format |
| certificates.cert | string | `nil` | Server certificate in base64 format |
| certificates.key | string | `nil` | Private key in base64 format |
| certificates.password | string | `nil` | Optional private key passwort |
| certificates.extraCerts | list | `[]` | List of extra certificates that will be mounted to the container into /ssl and can be used for custom/advanced configuration (see: options.customConfig) |
| certificates.extraCerts[].name | string | `nil` | Name of the certificate (will be the filename of the mounted certificate - i.e.: /ssl/{name}) |
| certificates.extraCerts[].cert | string | `nil` | The certificate content in base64 format |
| extraSecrets | list | `[]` | A list of additional existing secrets that will be mounted into the container |
| extraSecrets[].name | string | `nil` | Name of the existing K8s secret |
| extraSecrets[].mountPath | string | `nil` | Mount path where the secret should be mounted into the container (f.e. /mysecretfolder) |

## RabbitMQ plugin base parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| managementPlugin.enabled | bool | `true` | Enable management UI plugin with default configuration |
| managementPlugin.tcp.port | int | `15672` | Management UI port |
| prometheusPlugin.enabled | bool | `true` | Enable prometheus monitoring plugin with default configuration |
| prometheusPlugin.tcp.port | int | `15692` | Prometheus plugin TCP port |
| k8sPeerDiscoveryPlugin.enabled | bool | `true` | Enable K8s peer discovery plugin for a RabbitMQ HA-cluster with default configuration |
| k8sPeerDiscoveryPlugin.addressType | string | `"hostname"` | K8s peer discovery plugin address type (hostname or ip) |

## RabbitMQ custom configuration parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| customConfig | string | `nil` | Custom configuration entries for rabbitmq.conf (see https://www.rabbitmq.com/configure.html#config-file) |
| customAdvancedConfig | string | `nil` | Custom advanced configuration entries for advanced.config (see https://www.rabbitmq.com/configure.html#advanced-config-file) |
