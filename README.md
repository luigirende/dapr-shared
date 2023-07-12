# Dapr Ambient

Dapr Ambient allows you to create Dapr Applications using the `daprd` Sidecar as a Kubernetes `Daemonset` or `Deployment`. This enables other use cases where Sidecars are not the best option.

By running `daprd` as a Kubernetes `DaemonSet` (or as a `Deployment`) the `daprd` container will be running in each Kubernetes Node, reducing the network hops between the applications and Dapr.

If you need multiple Dapr Applications you can deploy this chart multiple times using different `ambient.appId`s.

## Getting Started

Before you install Dapr Ambient, please make sure you have Dapr installed in your cluster. 

If you want to get started with Dapr Ambient you can easily create a new Dapr Ambient instance by install the oficial Helm Chart:

```
helm install my-ambient-dapr-ambient oci://registry-1.docker.io/daprio/dapr-ambient-chart --set ambient.appId=<DAPR_APP_ID> --set ambient.remoteURL=<REMOTE_URL>
```

If you want to take a look at a step by step tutorial using some applications and interacting with Dapr Components check out the [step-by-step tutorial using Kubernetes KinD here](tutorial/README.md).

## From Source

To deploy this chart from source you can run from inside the `chart/dapr-ambient` directory:

```
helm install my-ambient . --set ambient.appId=<DAPR_APP_ID> --set ambient.channelAddress=<REMOTE_URL> 

```

Where `<DAPR_APP_ID>` is the Dapr App Id that you can use in your components (for example for scopes)

and `<REMOTE_URL>` is a reachable URL where `dapr-ambient` will forward notifications received by the Dapr sidecar.

Future versions might include forwarding notifications to multiple remote URLs.

## Customize Dapr Ambient
Is possible to customize Dapr Ambient using custom Helm values.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ambient.controlPlane.namespace | string | `"dapr-system"` | Namespace where Dapr Control Plane is. |
| ambient.daprd.apiLogging.enabled | bool | `true` | Enables API logging for the daprd. |
| ambient.daprd.app.protocol | string | `"http"` | Dapr which protocol your application is using. Valid options are `http`` and `grpc``. |
| ambient.daprd.grpcPort | int | `50001` | gRPC port for the Dapr Internal API to listen on. |
| ambient.daprd.httpPort | int | `3500` | The HTTP port for the Dapr API. |
| ambient.daprd.image.name | string | `"daprd"` | Daprd image. |
| ambient.daprd.image.pullPolicy | string | `"Always"` | Daprd image pull policy. |
| ambient.daprd.image.registry | string | `"docker.io/daprio"` | Daprd image registry. |
| ambient.daprd.image.tag | string | `"1.11.0"` | Daprd image version. |
| ambient.daprd.internalGrpcPort | int | `50002` | gRPC port for the Dapr Internal API to listen on. |
| ambient.daprd.listenAddresses | string | `"0.0.0.0"` | Comma separated list of IP addresses that daprd will listen to. Defaults to all in standalone mode. Defaults to [::1],127.0.0.1 in Kubernetes. To listen to all IPv4 addresses, use 0.0.0.0. To listen to all IPv6 addresses, use [::]. |
| ambient.daprd.metrics.enabled | bool | `true` | Enable prometheus metric. |
| ambient.daprd.metrics.port | int | `9090` | Sets the port for the sidecar metrics server. |
| ambient.daprd.mtls.enabled | bool | `false` | Enables automatic mTLS for daprd to daprd communication channels. |
| ambient.daprd.publicPort | int | `3501` | The HTTP public port for the Dapr API. |
| ambient.daprd.token | string | `""` | Dapr API token to use for token based API authentication. |
| ambient.deployment.replicas | int | `1` | The quantity of replicas. This property is set only when `ambient.strategy` is equal to `deployment` |
| ambient.initContainer.image.name | string | `"dapr-ambient"` | The dapr-ambient image name. |
| ambient.initContainer.image.pullPolicy | string | `"Always"` | The init container pull policy. |
| ambient.initContainer.image.registry | string | `"docker.io/matheuscruzdev"` | The dapr-ambient image registry. |
| ambient.initContainer.image.tag | string | `"latest"` | The dapr-ambient-init image tag. |
| ambient.initContainer.token | string | `""` | The dapr API token. |
| ambient.log.json | bool | `true` | The daprd log format. |
| ambient.log.level | string | `"info"` | The daprd log level. |
| ambient.remotePort | int | `0` | The remote port. |
| ambient.service.type | string | `"ClusterIP"` | The daprd service type. |
| ambient.serviceAccount.annotations | object | `{}` |  |
| ambient.serviceAccount.create | bool | `true` | Allows the option to create or not the service account. |
| ambient.serviceAccount.name | string | `""` | Kubernetes Service Account name. |
| ambient.strategy | string | `"daemonset"` | The default strategy to run dapr in ambient mode. Possible values `daemonset`, `deployment`. |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)

### Building from source

I've used the [CNCF `ko` project](https://ko.build/) to build multiplatform images for the proxy.
You can run the following command to build containers for the `dapr-ambient` proxy:

```
ko build --platform=linux/amd64,linux/arm64
```

