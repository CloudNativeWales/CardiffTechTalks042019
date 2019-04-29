# Cardiff Tech Talks: Use Kubernetes to deploy and scale applications

[![Join the chat at https://gitter.im/cloudnativewales/CardiffTechTalks042019](https://badges.gitter.im/cloudnativewales/CardiffTechTalks042019.svg)](https://gitter.im/cloudnativewales/CardiffTechTalks042019?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## Description

More information for the meetup can be found [here](https://www.meetup.com/Cardiff-Tech-Talk/events/260506089/).

## Setup

We're going to offer 3 different ways to run a cluster:

* [Minikube](minikube/README.md)
* [Docker: Kubernetes](docker/README.md)
* [AKS](ask/README.md)

You're more than welcome to use another service if you already know how to.

## Steps

### Create a Pod and a Service

* Save the following to _apple.yaml_:

```yaml
---
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: denhamparry/apple:1.0.0
---
kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - port: 3000
```

* Send the file to Kubernetes:

```bash
$ kubectl apply -f apple.yaml
```

## Create another Pod and Service

* Save the following to _banana.yaml_:

```bash
---
kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: denhamparry/banana:1.0.0
---
kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 3000
```

* Send the file to Kubernetes:

```bash
$ kubectl apply -f apple.yaml
```

## Create an Ingress

* Save the following to _ingress.yaml_:

```yaml
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: app
  annotations:
    kubernetes.io/ingress.class: nginx
    ingress.kubernetes.io/rewrite-target: '/'
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /apple
        backend:
          serviceName: apple-service
          servicePort: 3000
      - path: /banana
        backend:
          serviceName: banana-service
          servicePort: 3000
      - path: /
        backend:
          serviceName: apple-service
          servicePort: 3000
```

* Send the file to Kubernetes:

```bash
$ kubectl apply -f ingress.yaml
```

## Test your cluster

* Using `curl`:

```bash
$ curl demo.local
<html style="background-color:#8db600 ">
  <head>
    <title>v1.0.0</title>
  </head>
  <body style="display:flex;align-items:center;justify-content:center;color:#FFFFFF;font-family:sans-serif;font-size:6rem;margin:0;letter-spacing:-0.1em">
    <h1>Apple</h1>
  </body>
</html>
$ curl demo.local/apple
<html style="background-color:#8db600 ">
  <head>
    <title>v1.0.0</title>
  </head>
  <body style="display:flex;align-items:center;justify-content:center;color:#FFFFFF;font-family:sans-serif;font-size:6rem;margin:0;letter-spacing:-0.1em">
    <h1>Apple</h1>
  </body>
</html>
$ curl demo.local/banana
<html style="background-color:#ffe135 ">
  <head>
    <title>v1.0.0</title>
  </head>
  <body style="display:flex;align-items:center;justify-content:center;color:#000000;font-family:sans-serif;font-size:6rem;margin:0;letter-spacing:-0.1em">
    <h1>Banana</h1>
  </body>
</html>
```

* Using your browser:

  * http://demo.local
  * http://demo.local/apple
  * http://demo.local/banana

## Clean up

* [Minikube](minikube/CLEANUP.md)
* [Docker](docker/CLEANUP.md)