# Introduction
This chart is an updated clone of the official EMQX helm-chart found here - https://github.com/emqx/emqx-rel/tree/master/deploy/charts/emqx
Appart of its ability to bootstrap an emqx deployment of X nodes on a Kubernetes cluster, using the Helm package manager, it also takes care of some configuration files, ingress services, consul registration and etc...

# Prerequisites
+ Kubernetes 1.6+
+ Helm

# Installing the Chart
To install the chart with release name `my-emqx`:

+   From github
    ```
    $ git clone https://github.com/robomir/helm-chart.git
    $ cd helm-chart/emqx-broker/
    $ helm install my-emqx .
    ```

+   From chart repos
    ```
    helm repo add robo-helm https://robomir.github.io/helm-chart/
    helm install my-emqx robo-helm/emqx-broker
    ```

# Uninstalling the Chart
To uninstall/delete the `my-emqx` deployment:
```
$ helm del --purge my-emqx
$ kubectl del pvc emqx-data-my-emqx-0
$ kubectl del pvc emqx-data-my-emqx-1
$ kubectl del pvc emqx-data-my-emqx-2
$ kubectl del pvc emqx-data-my-emqx-3
$ kubectl del pvc emqx-data-my-emqx-4
```

# Configuration
The following table lists the configurable parameters of the emqx chart and their default values.

| Parameter  | Description | Default Value |
| ---        |  ---        | ---           |
| `replicaCount` | It is recommended to have odd number of nodes in a cluster, otherwise the emqx cluster cannot be automatically healed in case of net-split. | 3 |
| `image.repository` | EMQ X Image name | emqx/emqx |
| `image.pullPolicy`  | Global Docker registry secret names as an array | IfNotPresent |
| `persistence.enabled` | Enable emqx persistence using PVC | true |
| `persistence.storageClass` | Storage class of backing PVC | `nil` (uses alpha storage class annotation) |
| `persistence.existingClaim` | EMQ X data Persistent Volume existing claim name, evaluated as a template | "" |
| `persistence.accessMode` | PVC Access Mode for emqx volume | ReadWriteOnce |
| `persistence.size` | PVC Storage Request for emqx volume | 20Mi |
| `resources` | CPU/Memory resource requests 500m CPU; 500Mi RAM / limits 1 CPU ; 1G RAM | enabled: `true` |
| `nodeSelector` | Node labels for pod assignment | `beta.kubernetes.io/os: linux` |
| `tolerations` | Toleration labels for pod assignment | `[]` |
| `affinity` | Map of node/pod affinities | `{}` |
| `service.type`  | emqx cluster service type. | LoadBalance |
| `ingress.enabled` | ingress configuration | false |
| `ingress.domainname` | release name is appended to ingress domainname to form the hostname for the ingress | `exmp-env.example.com` |
| `ingress.path` | ingress path| `/` |
| `ingress.tls` | ingress tls config | none |
| `ingress.annotations` | ingress annottations | `kubernetes.io/ingress.class: nginx` |
| `emqxConfig` | emqx configuration item, see the [documentation](https://github.com/emqx/emqx-docker#emq-x-configuration) | enabled: `true` |
| `emqxAclConfig` | acl configuration item, see the [documentation](https://docs.emqx.io/tutorial/v4/en/security/acl.html)| enabled: `true` |
| `emqxAuthConfig` | auth plugin configuration item, see the [documentation](https://github.com/emqx/emqx-auth-username) | enabled: `true` |
| `emqxStatsdConfig` | statsd plugin configuration item, see the [documentation](https://github.com/emqx/emqx-statsd) | enabled: `true` |
| `emqxPluginConfig` | plugin configuration item, see the [documentation](https://docs.emqx.io/tutorial/v4/en/config/plugins.html) | enabled: `true` |
