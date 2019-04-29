# Cardiff Tech Talks: Use Kubernetes to deploy and scale applications

More information for the meetup can be found [here](https://www.meetup.com/Cardiff-Tech-Talk/events/260506089/).

## Setup

We're going to offer 3 different ways to run a cluster:

* [Minikube](minikube/README.md)
* [Docker: Kubernetes](docker/README.md)
* [AKS](ask/README.md)

You're more than welcome to use another service if you already know how to.

## Steps

### Create a Pod and a Service

* Save the following to _apple.yaml_.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple"

---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - port: 5678 # Default port for image
```

```bash
$ kubectl apply -f apple.yaml

```

## Create another Pod and Service

```bash
curl $(minikube ip)/apple
```