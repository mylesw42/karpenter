# karpenter

A Helm chart for Karpenter, an open-source node provisioning project built for Kubernetes.

![Version: 0.31.0](https://img.shields.io/badge/Version-0.31.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.31.0](https://img.shields.io/badge/AppVersion-0.31.0-informational?style=flat-square)

## Documentation

For full Karpenter documentation please checkout [https://karpenter.sh](https://karpenter.sh/docs/).

## Installing the Chart

You can follow the detailed installation instruction in the [documentation](https://karpenter.sh/docs/getting-started/getting-started-with-karpenter/#install) which covers the Karpenter prerequisites and installation options. The outcome of these instructions should result in something like the following command.

```bash
helm upgrade --install --namespace karpenter --create-namespace \
  karpenter oci://public.ecr.aws/karpenter/karpenter \
  --version v0.31.0 \
  --set serviceAccount.annotations.eks\.amazonaws\.com/role-arn=${KARPENTER_IAM_ROLE_ARN} \
  --set settings.aws.clusterName=${CLUSTER_NAME} \
  --set settings.aws.clusterEndpoint=${CLUSTER_ENDPOINT} \
  --set settings.aws.defaultInstanceProfile=KarpenterNodeInstanceProfile-${CLUSTER_NAME} \
  --set settings.aws.interruptionQueueName=${CLUSTER_NAME} \
  --wait
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| additionalAnnotations | object | `{}` | Additional annotations to add into metadata. |
| additionalClusterRoleRules | list | `[]` | Specifies additional rules for the core ClusterRole. |
| additionalLabels | object | `{}` | Additional labels to add into metadata. |
| affinity | object | `{"nodeAffinity":{"requiredDuringSchedulingIgnoredDuringExecution":{"nodeSelectorTerms":[{"matchExpressions":[{"key":"karpenter.sh/provisioner-name","operator":"DoesNotExist"}]}]}},"podAntiAffinity":{"requiredDuringSchedulingIgnoredDuringExecution":[{"topologyKey":"kubernetes.io/hostname"}]}}` | Affinity rules for scheduling the pod. If an explicit label selector is not provided for pod affinity or pod anti-affinity one will be created from the pod selector labels. |
| controller.env | list | `[]` | Additional environment variables for the controller pod. |
| controller.envFrom | list | `[]` |  |
| controller.errorOutputPaths | list | `["stderr"]` | Controller errorOutputPaths - defaults to stderr only (Deprecated: Use logConfig.errorOutputPaths instead) |
| controller.extraVolumeMounts | list | `[]` | Additional volumeMounts for the controller pod. |
| controller.healthProbe.port | int | `8081` | The container port to use for http health probe. |
| controller.image.digest | string | `"sha256:d29767fa9c5c0511a3812397c932f5735234f03a7a875575422b712d15e54a77"` | SHA256 digest of the controller image. |
| controller.image.repository | string | `"public.ecr.aws/karpenter/controller"` | Repository path to the controller image. |
| controller.image.tag | string | `"v0.31.0"` | Tag of the controller image. |
| controller.logLevel | string | `""` | Controller log level, defaults to the global log level (Deprecated: Use logConfig.logLevel.controller instead) |
| controller.metrics.port | int | `8000` | The container port to use for metrics. |
| controller.outputPaths | list | `["stdout"]` | Controller outputPaths - defaults to stdout only (Deprecated: Use logConfig.outputPaths instead) |
| controller.resources | object | `{}` | Resources for the controller pod. |
| controller.sidecarContainer | list | `[]` | Additional sidecarContainer config |
| controller.sidecarVolumeMounts | list | `[]` | Additional volumeMounts for the sidecar - this will be added to the volume mounts on top of extraVolumeMounts |
| dnsConfig | object | `{}` | Configure DNS Config for the pod |
| dnsPolicy | string | `"Default"` | Configure the DNS Policy for the pod |
| extraVolumes | list | `[]` | Additional volumes for the pod. |
| fullnameOverride | string | `""` | Overrides the chart's computed fullname. |
| hostNetwork | bool | `false` | Bind the pod to the host network. This is required when using a custom CNI. |
| imagePullPolicy | string | `"IfNotPresent"` | Image pull policy for Docker images. |
| imagePullSecrets | list | `[]` | Image pull secrets for Docker images. |
| logConfig | object | `{"enabled":true,"errorOutputPaths":["stderr"],"logEncoding":"console","logLevel":{"controller":"debug","global":"debug","webhook":"error"},"outputPaths":["stdout"]}` | Log configuration |
| logConfig.enabled | bool | `true` | Whether to enable provisioning and mounting the log ConfigMap |
| logConfig.errorOutputPaths | list | `["stderr"]` | Log errorOutputPaths - defaults to stderr only |
| logConfig.logEncoding | string | `"console"` | Log encoding - defaults to console - must be one of 'json', 'console' |
| logConfig.logLevel | object | `{"controller":"debug","global":"debug","webhook":"error"}` | Component-based log configuration |
| logConfig.outputPaths | list | `["stdout"]` | Log outputPaths - defaults to stdout only |
| logEncoding | string | `"console"` | Global log encoding (Deprecated: Use logConfig.logEncoding instead) |
| logLevel | string | `"debug"` | Global log level |
| nameOverride | string | `""` | Overrides the chart's name. |
| nodeSelector | object | `{"kubernetes.io/os":"linux"}` | Node selectors to schedule the pod to nodes with labels. |
| podAnnotations | object | `{}` | Additional annotations for the pod. |
| podDisruptionBudget.maxUnavailable | int | `1` |  |
| podDisruptionBudget.name | string | `"karpenter"` |  |
| podLabels | object | `{}` | Additional labels for the pod. |
| priorityClassName | string | `"system-cluster-critical"` | PriorityClass name for the pod. |
| replicas | int | `2` | Number of replicas. |
| revisionHistoryLimit | int | `10` | The number of old ReplicaSets to retain to allow rollback. |
| serviceAccount.annotations | object | `{}` | Additional annotations for the ServiceAccount. |
| serviceAccount.create | bool | `true` | Specifies if a ServiceAccount should be created. |
| serviceAccount.name | string | `""` | The name of the ServiceAccount to use. If not set and create is true, a name is generated using the fullname template. |
| serviceMonitor.additionalLabels | object | `{}` | Additional labels for the ServiceMonitor. |
| serviceMonitor.enabled | bool | `false` | Specifies whether a ServiceMonitor should be created. |
| serviceMonitor.endpointConfig | object | `{}` | Endpoint configuration for the ServiceMonitor. |
| settings | object | `{"aws":{"assumeRoleARN":"","assumeRoleDuration":"15m","clusterCABundle":"","clusterEndpoint":"","clusterName":"","defaultInstanceProfile":"","enableENILimitedPodDensity":true,"enablePodENI":false,"interruptionQueueName":"","isolatedVPC":false,"tags":null,"vmMemoryOverheadPercent":0.075},"batchIdleDuration":"1s","batchMaxDuration":"10s","featureGates":{"driftEnabled":false}}` | Global Settings to configure Karpenter |
| settings.aws | object | `{"assumeRoleARN":"","assumeRoleDuration":"15m","clusterCABundle":"","clusterEndpoint":"","clusterName":"","defaultInstanceProfile":"","enableENILimitedPodDensity":true,"enablePodENI":false,"interruptionQueueName":"","isolatedVPC":false,"tags":null,"vmMemoryOverheadPercent":0.075}` | AWS-specific configuration values |
| settings.aws.assumeRoleARN | string | `""` | Role to assume for calling AWS services. |
| settings.aws.assumeRoleDuration | string | `"15m"` | Duration of assumed credentials in minutes. Default value is 15 minutes. Not used unless aws.assumeRoleARN set. |
| settings.aws.clusterCABundle | string | `""` | Cluster CA bundle for TLS configuration of provisioned nodes. If not set, this is taken from the controller's TLS configuration for the API server. |
| settings.aws.clusterEndpoint | string | `""` | Cluster endpoint. If not set, will be discovered during startup (EKS only) |
| settings.aws.clusterName | string | `""` | Cluster name. |
| settings.aws.defaultInstanceProfile | string | `""` | The default instance profile to use when launching nodes |
| settings.aws.enableENILimitedPodDensity | bool | `true` | Indicates whether new nodes should use ENI-based pod density DEPRECATED: Use `.spec.kubeletConfiguration.maxPods` to set pod density on a per-provisioner basis |
| settings.aws.enablePodENI | bool | `false` | If true then instances that support pod ENI will report a vpc.amazonaws.com/pod-eni resource |
| settings.aws.interruptionQueueName | string | `""` | interruptionQueueName is disabled if not specified. Enabling interruption handling may require additional permissions on the controller service account. Additional permissions are outlined in the docs. |
| settings.aws.isolatedVPC | bool | `false` | If true then assume we can't reach AWS services which don't have a VPC endpoint This also has the effect of disabling look-ups to the AWS pricing endpoint |
| settings.aws.tags | string | `nil` | The global tags to use on all AWS infrastructure resources (launch templates, instances, etc.) across node templates |
| settings.aws.vmMemoryOverheadPercent | float | `0.075` | The VM memory overhead as a percent that will be subtracted from the total memory for all instance types |
| settings.batchIdleDuration | string | `"1s"` | The maximum amount of time with no new ending pods that if exceeded ends the current batching window. If pods arrive faster than this time, the batching window will be extended up to the maxDuration. If they arrive slower, the pods will be batched separately. |
| settings.batchMaxDuration | string | `"10s"` | The maximum length of a batch window. The longer this is, the more pods we can consider for provisioning at one time which usually results in fewer but larger nodes. |
| settings.featureGates | object | `{"driftEnabled":false}` | Feature Gate configuration values. Feature Gates will follow the same graduation process and requirements as feature gates in Kubernetes. More information here https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/#feature-gates-for-alpha-or-beta-features |
| settings.featureGates.driftEnabled | bool | `false` | driftEnabled is in ALPHA and is disabled by default. Setting driftEnabled to true enables the drift deprovisioner to watch for drift between currently deployed nodes and the desired state of nodes set in provisioners and node templates |
| strategy | object | `{"rollingUpdate":{"maxUnavailable":1}}` | Strategy for updating the pod. |
| terminationGracePeriodSeconds | string | `nil` | Override the default termination grace period for the pod. |
| tolerations | list | `[{"key":"CriticalAddonsOnly","operator":"Exists"}]` | Tolerations to allow the pod to be scheduled to nodes with taints. |
| topologySpreadConstraints | list | `[{"maxSkew":1,"topologyKey":"topology.kubernetes.io/zone","whenUnsatisfiable":"ScheduleAnyway"}]` | Topology spread constraints to increase the controller resilience by distributing pods across the cluster zones. If an explicit label selector is not provided one will be created from the pod selector labels. |
| webhook.enabled | bool | `true` | Whether to enable the webhooks and webhook permissions. |
| webhook.logLevel | string | `"error"` | Webhook log level (Deprecated: Use logConfig.logLevel.webhook instead) |
| webhook.metrics.port | int | `8001` | The container port to use for webhook metrics. |
| webhook.port | int | `8443` | The container port to use for the webhook. |

