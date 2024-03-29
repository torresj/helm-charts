# Cloud config

Cloud config server based on [Spring cloud config server](https://cloud.spring.io/spring-cloud-config/reference/html/). Spring Cloud Config provides server-side and client-side support for externalized configuration in a distributed system. With the Config Server, you have a central place to manage external properties for applications across all environments. The concepts on both client and server map identically to the Spring Environment and PropertySource abstractions, so they fit very well with Spring applications but can be used with any application running in any language. As an application moves through the deployment pipeline from dev to test and into production, you can manage the configuration between those environments and be certain that applications have everything they need to run when they migrate. The default implementation of the server storage backend uses git, so it easily supports labelled versions of configuration environments as well as being accessible to a wide range of tooling for managing the content. It is easy to add alternative implementations and plug them in with Spring configuration.

## Example

```bash
$ helm repo add torresj https://charts.torresj.com
$ helm install my-release torresj/cloud-config
```

## Introduction

This chart bootstraps a [Spring cloud config server](https://cloud.spring.io/spring-cloud-config/reference/html/) replication deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.18+
- Helm 2.16+ or Helm 3.0-beta3+

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm repo add torresj https://charts.torresj.com
$ helm install my-release torresj/cloud-config
```

These commands deploy cloud config server on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release --purge
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Docker image

This chart use by default a custom docker image [spring-cloud-config-server](https://hub.docker.com/repository/docker/torresjb/spring-cloud-config-server) published in [docker hub](https://hub.docker.com/). This image has the following environment variables:

**`APP_VERSION`**

This variable is mandatory and set the spring cloud config server version.

**`USER_NAME`**

This variable is mandatory and set the spring security user to connect with server.

**`USER_PASSWORD`**

This variable is mandatory and set the spring security password to connect with server.

**`GIT_URI`**

This variable is mandatory and set git repository to get configurations.

**`GIT_LABEL`**

This variable is optional and set git repository label. Default value is `main`.

**`GIT_USER`**

This variable set git user. It is mandatory if git repository is private.

**`GIT_PASSWORD`**

This variable set git password. It is mandatory if git repository is private.

**`SPRING_BOOT_ADMIN_ENABLED`**

Variable to enable connect with a spring boot admin server. Defaul value is `false`.

**`SPRING_BOOT_ADMIN_URL`**

If spring boot admin server is enabled, it is mandatory set the url of the server.

**`SPRING_BOOT_ADMIN_USERNAME`**

If spring boot admin server is enabled, it is mandatory set the username to connect with admin server.

**`SPRING_BOOT_ADMIN_USERNAME`**

If spring boot admin server is enabled, it is mandatory set the password to connect with admin server.

## Parameters

The following tables lists the configurable parameters of the cloud config chart and their default values.

| Parameter                                 | Description                                                                                               | Default                                               |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `image.repository`                        | Cloud config Image repository                                                                             | `torresj/cloud-config`                                |
| `image.name`                              | Cloud config Image name                                                                                   | `spring-cloud-config-server`                          |
| `image.version`                           | Cloud config Image version                                                                                | Current latest version `0.1.3`                        |
| `image.pullPolicy`                        | Cloud config image pull policy                                                                            | `IfNotPresent`                                        |
| `nameOverride`                            | String to partially override cloud-config.fullname template with a string (will prepend the release name) | `nil`                                                 |
| `fullnameOverride`                        | String to fully override cloud-config.fullname template with a string                                     | `nil`                                                 |
| `replicaCount`                            | K8s replicas                                                                                              | `IfNotPresent`                                        |
| `service.type`                            | K8s service type                                                                                          | `ClusterIP`                                           |
| `service.port`                            | K8s service port                                                                                          | `8888`                                                |
| `config.server.username`                  | Cloud config server user for http request                                                                 | `user`                                                |
| `config.server.password`                  | Cloud config server password for http request                                                             | `password`                                            |
| `git.credentials.enabled`                 | If git repository is private, git credentials are required                                                | `false`                                               |
| `git.user`                                | Git user required if `git.credentials.enabled`=`true`                                                     | `nil`                                                 |
| `git.password`                            | Git password required if `git.credentials.enabled`=`true`                                                 | `nil`                                                 |
| `git.uri`                                 | Git uri repo with configuration files                                                                     | `https://github.com/spring-cloud-samples/config-repo` |
| `git.label`                               | Git label                                                                                                 | `main`                                                |
| `ingress.enabled`                         | Enable K8s ingress                                                                                        | `false`                                               |
| `ingress.controller.class`                | Ingress controller class                                                                                  | `nginx`                                               |
| `ingress.host`                            | Ingress host                                                                                              | `cloud-config.local`                                  |
| `ingress.path`                            | Ingress path                                                                                              | `/`                                                   |
| `ingress.tls.enabled`                     | Enable tls for K8s ingress                                                                                | `false`                                               |
| `ingress.tls.cert_manager.cluster_issuer` | Cluster issuer to be used with cert manager                                                               | `nil`                                                 |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set git.uri=https://github.com/spring-cloud-examples \
    torresj/cloud-config
```

The above command sets the git repository to `https://github.com/spring-cloud-examples`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f values.yaml torresj/cloud-config
```

> **Tip**: You can use the default [values.yaml](values.yaml)
