# Spring Cloud Netflix Eureka


## Eureka

Eureka is service discovery server in [Spring Cloud Netflix](http://cloud.spring.io/spring-cloud-netflix/) . 

## Introduction

This chart bootstraps a two node Eureka deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. 

## Installing the Chart

To install the chart with the release name `myeureka`:

```bash
$ helm install --name myeureka incubator/ack-springcloud-eureka
```

The command deploys Eureka on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

Each eureka server instance are equiped with a headless service. If you set `sevice.enabled=true`, then a service is also deployed. Suppose `replicaCount=2` and `service.enabled=true`, user will get three services (two headless, one normal). 



```
NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)
myeureka-ack-springcloud-eureka-headless-svc-0   ClusterIP      172.21.7.149   <none>         8761/TCP
myeureka-ack-springcloud-eureka-headless-svc-1   ClusterIP      172.21.13.83   <none>         8761/TCP
myeureka-ack-springcloud-eureka-svc              LoadBalancer   172.21.3.184   aa.bb.cc.dd    8761:32737/TCP
```



Eureka server can be access by visiting headless service internally, or by visiting normal service internally or externally.



## Eureka server dashboard

User can access Eureka dashboard port 8761 http://eureka-svc:8761/.  Service port number can be configured values.yml, details in configuration section.



## Client connect to server

To connect to Eureka server, client must set Eureka server address into configuration file. The configuration file can be in .yaml or .properties. Below example shows how to configue Eureka server address in `application.yaml` file.



Access eureak using headless services:

```yaml
eureka:
  client:
    serviceUrl:
      zefaultZone: http://myeureka-ack-springcloud-eureka-headless-svc-0.default.svc.cluster.local/eureka,http://myeureka-ack-springcloud-eureka-headless-svc-1.default.svc.cluster.local/eureka
```



Access eureak using service:

```yaml
eureka:
  client:
    serviceUrl:
      zefaultZone: http://myeureka-ack-springcloud-eureka-svc.default.svc.cluster.local:8761/eureka
```





## Uninstalling the Chart

To uninstall/delete the `myeureka` deployment completely:

```bash
$ helm delete --purge myeureka
```

The command removes all the Kubernetes components associated with the chart and deletes the release.



## Configuration

The following tables lists the configurable parameters of Eureka chart and their default values.

| Parameter                            | Description                                                  | Default                                    |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------ |
| `replicaCount` | Eureka server instance number. 1 as single node deployment, 2 as two node high available deployment. | 2 |
| `image.repository`                     | `eureka` image repository                                                    |                                            |
| `image.tag`                           | `mysql` image tag.                                           | Most recent release                        |
| `image.pullPolicy`                    | Image pull policy                                            | `IfNotPresent`                             |
| `service.enabled`                  | If the eureka server can be exposed by a service                                | `true`                                  |
| `service.type`        | Service type. | `LoadBalancer`                          |
| `service.externalPort`  | Service port.                             | `8761`                                  |
| `service.internalPort` | Eureka listening port. Please do NOT change this setting. | `8761`                                  |
| `management.endpointsEnabled` | Expose management endpoints | `true` |

