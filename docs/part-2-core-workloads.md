# Kubernetes 101 - Part 2: Core workloads

In the previous part we stood up a local Kubernetes cluster.

## Deploying your first pod
Let’s deploy an application into the cluster. To do this, we are going to create a pod, based on a Docker image.

Create a YAML file for your pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    name: my-app
spec:
  containers:
  - name: my-app
    image: nginx
    ports:
    - containerPort: 80
```

Now let’s create the pod object in Kubernetes:

	kubectl apply -f my-pod.yaml

Let’s check the status of your pod:

	kubectl get pods

If you want more information, you can use:

	kubectl describe pod my-pod

This gives you a human-readable description of the object.

Show how to view logs.
Connecting to your pod
Now, by itself, this pod is not very exciting. Whilst it’s running, it’s not accessible to the outside world. What we need is to expose the internal port on which it listens for traffic via a Service.

Let’s add a service definition for your pod:

```
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  ports:
  - port: 8080
    targetPort: 80
    protocol: TCP
  selector:
    app: my-pod
```

Notice how the service is bound to your pod using its metadata. This provides you with loose coupling between your objects.

Add the service:

	kubectl apply -f my-service.yaml

Let’s check the status of the service:

	kubectl get services

When your service is active, you should be able to access it on the host port you specified, using the IP address of minikube:

	minikube ip

You can see your service in your web browser at http://<minikube IP>:<service port>

## Fault tolerance

In the real world, hardware and software are never perfect. A server might fail, or become inaccessible over the network, or your application could crash. Distributed systems need to be resilient to various failure conditions. The way in which Kubernetes provides this is via the concept of the ReplicaSet.

> Explanation and example of ReplicaSet and Deployment

Let's replace our Pod with a Deployment:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: my-app
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      restartPolicy: Always
      containers:
      - image: nginx:1.7.9
        name: my-app
        ports:
        - containerPort: 80
```

Apply it:

    kubectl apply -f my-deployment.yaml

Now the deployment controller will parse the Deployment object the state of the cluster will change as follows:

1. Deployment controller creates a ReplicaSet
2. Deployment controller scales the ReplicaSet to the desired count
3. ReplicaSet creates the desired number of Pods

Let's look for the objects we expect:

    kubectl get deployment
    kubectl get replicaset

And finally, let's see how our application is doing:

    kubectl get pods

You'll notice the deployment controller has reconciled the actual state (no pods running), with the desired state (a pod running with the given specification).

### Scaling up

In the real world, we will typically run more than one instance of an application.

Let's scale up our Deployment:

    vi my-deployment.yaml
    <edit replicas to a higher number>
    
    kubectl apply -f my-deployment.yaml

Just as before, the deployment controller notices the change in desired state, adjusts or creates a new ReplicSet, and our application has the desired number of instances.

Let's take a look:

    kubectl get pod

As you can see, we now have more than one 

### Load balancing

Using a Service with multiple Pods will result in the load being spread across all instances. The Service load balancer performs a simple round robin strategy.

### Rolling deployments

By default, Kubernetes performs rolling updates whenever a change is required to an application managed by the Deployment controller. This has a number of advantages including eliminating downtime and phasing the introduction of new versions of your application. The rolling update strategy can be customised to meet a number of different scenarios.

> See the [official documentation](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/) for a full explanation.
