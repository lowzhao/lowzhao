---
published: false
layout: post
author: eugene
categories: Kubernetes
tags:
  - kubernetes
---
## Kubernetes Cheat Sheet

## Kubernetes Hierarchy and Knowledge Check.

`Deployment` > `ReplicaSet` > `Pods` > `Container`

- Usually `deployment` decides how a pods would behave and where to get the container. 
- `Deployment`s are connected by `service`(and abstraction of pods and deployments) where each `service` can have [few types](https://kubernetes.io/docs/reference/kubernetes-api/service-resources/service-v1/#ServiceSpec) mainly `ClusterIP`, `ExternalName`, `LoadBalancer` and `NodePort`.
- The `service` types:
	- `ClusterIP`(default): open network in cluster (node cannot access)
    - `ExternalName`: acts like CNAME of DNS and alias
    - `LoadBalancer`: open network to cloud provider/external load balancer.
    - `NodePort`: open to every node.
- The `service` are able to route requests within the cluster from the port of the service to the target port of the deployment and pods.
- `selector` is a very important key in the config files, its used to identify the kubernetes components where it is related, ie, `deployment` finds `pods` through `selector` and `service` find `pods` through `selector`. `name` is also a similar idea, `Ingress` find `service` by the `name`.

### Get all
```bash
kubectl get all -A;
```

### Log
```bash
kubectl logs component/componentName -n namespace;
# OR
kubectl logs component componentName -n namespace;
```

### Set Current/Active Namespace
```bash
kubectl config set-context --current --namespace=namespace;
```

### Trigger a cronjob
```bash
kubectl create job --from=cronjob/<name of cronjob> <name of job> -n namespace;
```

\* I highly suggest learning the basics of the Kubernetes from this [Youtube by Kubernetes Tutorial for Beginners \[FULL COURSE in 4 Hours\] by TechWorld with Nana][Youtube by Kubernetes Tutorial for Beginners \[FULL COURSE in 4 Hours\] by TechWorld with Nana] although it is lengthy and read kubernetes components configuration from the [reference][Reference] instead of the [website](https://kubernetes.io/docs/home/) where kubernetes provided, they are lengthy and confusing. 

[Reference]: https://kubernetes.io/docs/reference/kubernetes-api/
[Youtube by Kubernetes Tutorial for Beginners \[FULL COURSE in 4 Hours\] by TechWorld with Nana]: https://www.youtube.com/watch?v=X48VuDVv0do