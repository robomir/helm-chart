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

+   From chart repo
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
| `persistence.enabled` | Enable emqx persistence using PVC | enabled: `true` |
| `persistence.storageClass` | Storage class of backing PVC | `nil` (uses alpha storage class annotation) |
| `persistence.existingClaim` | EMQ X data Persistent Volume existing claim name, evaluated as a template | "" |
| `persistence.accessMode` | PVC Access Mode for emqx volume | ReadWriteOnce |
| `persistence.size` | PVC Storage Request for emqx volume | 20Mi |
| `resources` | CPU/Memory resource requests/limits | {} |
| `nodeSelector` | node labels for pod assignment | `beta.kubernetes.io/os: linux` |
| `tolerations` | toleration labels for pod assignment | `[]` |
| `affinity` | map of node/pod affinities | `{}` |
| `service.type`  | emqx cluster service type. | ClusterIP |
| `ingress.enabled` | ingress configuration | enabled: `true` |
| `ingress.domainname` | release name is appended to ingress domainname to form the hostname for the ingress | `exmp-env.example.com` |
| `ingress.path` | ingress path| `/` |
| `ingress.tls` | ingress tls config | none |
| `ingress.annotations` | ingress annottations | `kubernetes.io/ingress.class: nginx` |
| `emqxConfig` | emqx configuration item, see the [documentation](https://github.com/emqx/emqx-docker#emq-x-configuration) | enabled: `true` |
| `emqxAclConfig` | acl configuration item, see the [documentation](https://docs.emqx.io/tutorial/v4/en/security/acl.html)| enabled: `true` |
| `emqxAuthConfig` | auth plugin configuration item, see the [documentation](https://github.com/emqx/emqx-auth-username) | enabled: `true` |
| `configReloader` | If you would like for a daemon to watch if some change happens in "acl.conf" and "emqx_auth_username.conf" secrets and also emqx-broker-env configMap. Once there are changes in any of the above the daemon will perform a rolling upgrade on the Statefulset | enabled: `true` |
