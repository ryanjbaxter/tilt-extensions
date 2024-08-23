# Tilt Extensions For SpringOne 2024

The extensions in this repo are for my session, Frustration Free K8S For Spring Developers that was given at SpringOne 2024.

While these extensions were useful for my demo, they are more generally useful for Spring Developers using Tilt to deploy Spring apps on Kubernetes.

## spring-cloud-k8s-discoveryserver

This extension can be used to deploy Spring Cloud Kubernetes Discovery Server to K8S using Tilt.

To use this extension do the following:
```
v1alpha1.extension_repo(name='tilt-extensions', url='https://github.com/ryanjbaxter/tilt-extensions')
v1alpha1.extension(name='spring-cloud-k8s-discoveryserver', repo_name='tilt-extensions', repo_path='spring-cloud-k8s-discoveryserver')
load('ext://spring-cloud-k8s-discoveryserver', 'discovery_server_deployment')
# This function call will deploy the Spring Cloud Kubernetes Discovery Server
discovery_server_deployment()
```

## spring-cloud-k8s-configserver

This extension can be used to deploy Spring Cloud Kubernetes Config Server to K8S using Tilt.

To use this extension do the following:
```
v1alpha1.extension_repo(name='tilt-extensions', url='https://github.com/ryanjbaxter/tilt-extensions')
v1alpha1.extension(name='spring-cloud-k8s-configserver', repo_name='tilt-extensions', repo_path='spring-cloud-k8s-configserver')
load('ext://spring-cloud-k8s-configserver', 'config_server_deployment')
# This function call will deploy the Spring Cloud Kubernetes Config Server
# config_server_deployment([path to config server configuration yaml)
config_server_deployment('./k8s/kubernetesconfigserver.yaml')
```
`./k8s/kubernetesconfigserver.yaml` will be place in a Config Map the Spring Cloud Kubernetes Config Server will pick that up.  Configuration placed in this YAML file can be used to configure the config server.

For example:

```
spring.cloud.config.server.git.uri: https://github.com/ryanjbaxter/s12024-config
management.endpoints.web.exposure.include: "*"
management.endpoint.env.show-values: "always"
```

## bp-remote-debug

This extension is useful for modifying a deployment resource that is deploying a container created using Cloud Native Buildpacks to enable remote debugging.
It adds the neccessary environment variables to enable remote debug when the container starts and also exposes a port (defaults to 5005) to connect the remote debugger to.

To use this extension do the following:
```
v1alpha1.extension_repo(name='tilt-extensions', url='https://github.com/ryanjbaxter/tilt-extensions')
v1alpha1.extension(name='bp-remote-debug', repo_name='tilt-extensions', repo_path='bp-remote-debug')
load('ext://bp-remote-debug', 'modify_jkube_kubernetes_yaml_for_remote_debug')
modify_jkube_kubernetes_yaml_for_remote_debug_blob([blob of deployment yaml])
```

## mysql

This extensions simplifies deploying a MySQL instance using a Helm chat from Bitnami.

To use this extension do the following:
```
v1alpha1.extension_repo(name='tilt-extensions', url='https://github.com/ryanjbaxter/tilt-extensions')
v1alpha1.extension(name='mysql', repo_name='tilt-extensions', repo_path='mysql')
load('ext://mysql', 'create_mysql_resource', 'add_service_binding')
create_mysql_resource([name of the database to create when deploying MySQL])
add_service_binding([YAML as stream])
```

`create_mysql_resource` deploys the MySQL database to K8S
`add_service_binding` will add the volume containing the service binding secret to `/var/svcbind/mysql`.

## all-extensions

This extension wraps all the other extensions in the repo and simplies using everything.

To use this extension do the following: 

```
v1alpha1.extension_repo(name='tilt-extensions', url='https://github.com/ryanjbaxter/tilt-extensions')
v1alpha1.extension(name='all-extensions', repo_name='tilt-extensions', repo_path='all-extensions')
load('ext://all-extensions', 'configmap_create', 'configmap_from_dict', 'modify_jkube_kubernetes_yaml_for_livereload',
'config_server_deployment', 'discovery_server_deployment', 'create_mysql_resource', 'add_service_binding',
'modify_jkube_kubernetes_yaml_for_remote_debug', 'modify_jkube_kubernetes_yaml_for_livereload_blob')
```


