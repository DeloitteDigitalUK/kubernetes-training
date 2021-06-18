# Knative

![Knative architecture](img/knative-arch.jpeg)

> Source: https://github.com/knative/docs/

Install core:

```
kubectl apply -f https://github.com/knative/serving/releases/download/v0.22.0/serving-crds.yaml
kubectl apply -f https://github.com/knative/serving/releases/download/v0.22.0/serving-core.yaml
```

Add Istio network plugin:

```
kubectl apply -f https://github.com/knative/net-istio/releases/download/v0.22.0/net-istio.yaml
```

Set up default domain:

```
curl -LOs https://github.com/knative/serving/releases/download/v0.22.0/serving-default-domain.yaml

sed -i '' 's/xip.io/nip.io/g' serving-default-domain.yaml
kubectl apply serving-default-domain.yaml
```

## The Knative Service

Let's create our first Knative Service (not to be confused with a Kubernetes Service object).

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: my-app
spec:
  template:
    spec:
      containerConcurrency: 25
      containers:
      - image: nginx:1.7.9
        name: my-app
        ports:
          - containerPort: 80
```

At first glance, this is very similar to the Pod we defined at the start of the course.

The difference is that the Knative Service comes with a huge number of benefits vs. a Pod:

| Feature                   | Pod                        | Knative Service         |
|---------------------------|----------------------------|-------------------------|
| Auto restart on crash     | App crash, same node only  | Loss of node, app crash |
| Auto-scaling              | Requires HPA               | Automatic               |
| Scaling metrics (default) | (via HPA) CPU, memory      | Requests per second     |
| Scale to zero             | Not supported              | Supported               |
| Rolling deployments       | Not supported              | Supported               |
