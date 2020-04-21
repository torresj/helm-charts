# Spring Boot Admin

Spring Boot Admin is a community project to manage and monitor your [Spring Boot ®](https://spring.io/projects/spring-boot) applications. The applications register with our Spring Boot Admin Client (via HTTP). The UI is just a Vue.js application on top of the Spring Boot Actuator endpoints.

## Example

```bash
$ helm repo add torresj https://charts.torresj.com
$ helm install my-release torresj/spring-boot-admin
``` 

## Introduction

This chart bootstraps a [Spring boot admin server](https://codecentric.github.io/spring-boot-admin/current/) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. This chart allow to use external configuration using [Spring cloud config server](https://cloud.spring.io/spring-cloud-config/reference/html/) instead of define envirnoment variables. You can use the [cloud config](https://github.com/torresj/helm-charts/tree/master/cloud-config) chart for set external configurations.


## Prerequisites

- Kubernetes 1.18+
- Helm 2.16+ or Helm 3.0-beta3+


## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm repo add torresj https://charts.torresj.com
$ helm install my-release torresj/spring-boot-admin
```

These commands deploy spring boot server on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release --purge
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Docker image

This chart use by default a custom docker image [spring-boot-admin](https://hub.docker.com/repository/docker/torresjb/spring-boot-admin) published in [docker hub](https://hub.docker.com/). This image has the following environment variables:

**`SPRING_CLOUD_CONFIG_ENABLED`**

This variable is mandatory and set the spring cloud config server to be used instead of environment varables. Defaul value is `false`.

**`SPRING_CLOUD_CONFIG_URL`**

This variable is mandatory if `SPRING_CLOUD_CONFIG_ENABLED` = `true`. Set the url for spring cloud config server.

**`SPRING_CLOUD_CONFIG_USERNAME`**

This variable is mandatory if `SPRING_CLOUD_CONFIG_ENABLED` = `true`. Set the username for spring cloud config server.

**`SPRING_CLOUD_CONFIG_PASSWORD`**

This variable is mandatory if `SPRING_CLOUD_CONFIG_ENABLED` = `true`. Set the password for spring cloud config server.

**`USER_NAME`**

This variable is mandatory and set the spring security name to connect with server.

**`USER_PASSWORD`**

This variable is mandatory and set the spring security password to connect with server.

**`TELEGRAM_ENABLED`**

This variable enable/disable telegram notifications. Defaul value is `false`.

**`TELEGRAM_TOKEN`**

This variable is mandatory if `TELEGRAM_TOKEN` = `true`. Set the token telegram service.

**`TELEGRAM_CHAT_ID`**

This variable is mandatory if `TELEGRAM_TOKEN` = `true`. Set the telegram chat id to send notifications.

## Parameters

The following tables lists the configurable parameters of the spring boot admin chart and their default values.

| Parameter                                   | Description                                                                                                                                                                                                                                                    | Default                                                           |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| `image.repository`                          | Spring boot admin Image repository                                                                                                                                                                                                                                  | `torresj/spring-boot-admin`                                                   |
| `image.name`                                | Spring boot admin Image name                                                                                                                                                                                                                                        | `spring-boot-admin`                                                   |
| `image.version`                             | Spring boot admin Image version                                                                                                                                                                                                                                        | Current latest version `0.1.3`                                                   |
| `image.pullPolicy`                          | Spring boot admin image pull policy                                                                                                                                                                                                                                 | `IfNotPresent`                                                   |
| `nameOverride`                              | String to partially override cloud-config.fullname template with a string (will prepend the release name)                                                                                                                                                             | `nil`                                                             |
| `fullnameOverride`                          | String to fully override spring-boot-admin.fullname template with a string                                                                                                                                                                                                 | `nil` 
| `replicaCount`                              | K8s replicas                                                                                                                       | `IfNotPresent`                                                   |
| `service.type`                               | K8s service type                                                                                                                                                                                                                                 | `ClusterIP`                                                   |
| `service.port`                               | K8s service port                                                                                                                                                                                                                                 | `8888`                                                   |
| `admin.username`                         | Spring boot admin server user for http request                                                                                                                                                                                                                                | `user`       |
| `admin.password`                     | Spring boot admin server password for http request                                                                                                                                                                                                                               | `password`                                                   |
| `telegram.enabled`                                    | Enable telegram notifications                                                                                                                                                                                                                           | `false`                                                    |
| `telegram.token`                                    | mandatory if `telegram.enabled` = `true`. Token telegram bot                                                                                                                                                                                                                       | `nil`                                                    |
| `telegram.chat`                                    | mandatory if `telegram.enabled` = `true`. Telegram chat id                                                                                                                                                                                                                        | `nil`                                                    |
| `ingress.enabled`                                    | Enable K8s ingress                                                                                                                                                                                                                            | `false`                                                    |
| `ingress.controller.class`                       | Ingress controller class                                                                                                                                                                                                                             | `nginx`                                                    |
| `ingress.host`                                    | Ingress host                                                                                                                                                                                                                            | `spring-boot-admin.local`                                                    |
| `ingress.path`                                    | Ingress path                                                                                                                                                                                                                            | `/`                                                    | 
| `ingress.tls.enabled`                                    | Enable tls for K8s ingress                                                                                                                                                                                                                           | `false`                                                    |
| `ingress.tls.cert_manager.cluster_issuer`                                    | Cluster issuer to be used with cert manager                                                                                                                                                                                                                            | `nil`                                                    |
| `cloud.config.enabled`                                    | Enable get properties from config server instead of environment variables                                                                                                                                                                                                                           | `false`                                                    |
| `cloud.config.url`                                    | mandatory if `cloud.config.enabled` = `true`. Cloud config server url                                                                                                                                                                                                                           | `nil`                                                    |
| `cloud.config.username`                                    | mandatory if `cloud.config.enabled` = `true`. Cloud config server username                                                                                                                                                                                                                         | `nil`                                                    |
| `cloud.config.password`                                    | mandatory if `cloud.config.enabled` = `true`. Cloud config server password                                                                                                                                                                                                                           | `nil`                                                    |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set cloud.config.enabled=true,cloud.config.url=https://config.example.com,\
    cloud.config.username=user,cloud.config.password=password \
    torresj/spring-boot-admin
```

The above command create a spring boot admin and add the spring cloud config server `https://config.example.com` to get properties.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f values.yaml torresj/spring-boot-admin
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Spring cloud config properties

Example of `spring-boot-admin-server.yml`:
	
	#Spring security credentials
	spring:
	    security:
	      user:
	        name: USER
	        password: PASSWORD
	  
	    #configs to give secured server info
	    boot:
	      admin:
	        client:
	          instance:
	            metadata:
	              user:
	                name: ${spring.security.user.name}
	                password: ${spring.security.user.password}
	        #Telegram notifications
	        notify:
	          custom:
	            telegram:
	              enabled: true
	              auth-token: 12345678:ABCDEFGHIJKLMNOPQRSTW
	              chat-id: 111111