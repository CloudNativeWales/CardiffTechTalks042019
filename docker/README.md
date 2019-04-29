# Docker for Mac - Kubernetes

* [Back home](../README.md)

## Install Docker Kubernetes

* [Best check their official documentation here](https://docs.docker.com/docker-for-mac/#kubernetes/).

## Test Setup

```bash
$ kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
KubeDNS is running at https://192.168.99.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

* **NOTE**: If you don't see this, give us a shout!

## Create an Ingress

```bash
$ kubectl create namespace ingress-nginx
namespace "ingress-nginx" created
$kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/provider/cloud-generic.yaml
service "ingress-nginx" created
```

* [Back home](../README.md)