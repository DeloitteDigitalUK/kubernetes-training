# Kubernetes 101 - Part 1: Set up

Before we get started, let’s get you set up.

## Prerequisites

- Mac running macOS Sierra or later
- A text editor (an IDE is also fine)
- Homebrew
- Docker for Mac 17.06 or later

We’re going to use Homebrew to install the following packages:

	brew install kubectl kubernetes-helm
	brew cask install minikube

Whilst we wait for brew to do its thing, let’s spend a little bit of time understanding what Kubernetes _is_ and what it’s _for_.

## What is Kubernetes?

To understand this, it’s necessary to think about how we run operational systems today (and have done historically).

> Explanation, covering:
> - Immutable vs. mutable
> - Declarative vs. imperative
> - Desired vs. actual state reconciliation

So, Kubernetes is a platform for deploying and running things reliably, repeatably and predictably. It also allows your team to move more quickly because it standardises many parts of the release and deployment process, meaning you have less variation and fewer things to learn.

### How does it do this?

Now we know what Kubernetes is for, let’s look at what it is.

> Explanation, covering:
> - Masters and workers
> - A set of coordinated, but independent components
> - Controllers
> - API server
> - Proxy
> - kubelet
> - Use of containers (workloads as well as system)

By now, your installation should be complete. Let’s bring up your cluster.

## Bringing up a (local) cluster

Now you have the tools installed, we’re going to bring up a Kubernetes cluster, running on your local machine.

	minikube start --kubernetes-version 1.12.7

### What did we just do?

You used minikube, which is a way of starting a single-node development cluster of Kubernetes, right on your machine. Under the covers, it creates a Virtual Machine, running a lightweight distribution of Linux, and uses the kubeadm tool to bootstrap the components such as the API server, proxy and kubelet.

## Exploring your cluster

Now you have a cluster up and running, let’s explore. We’re going to use the kubectl command line tool we installed earlier to explore the Kubernetes API.

First of all, let’s start with some basics:

	kubectl version

This shows you the client and server version. Don’t worry if they’re different - Kubernetes adheres to the Semantic Versioning standard to allow forwards and backwards compatibility between +/- 2 minor versions.

Let’s get some information about the cluster:

	kubectl cluster-info

...and the components:

	kubectl get componentstatuses

Let’s look at the nodes in our cluster:

	kubectl get nodes

You can see we have a single node running everything.

### Concepts

> Explanation of:
> 
> - Objects (represented as YAML or JSON)
> - Namespaces (including kube-system)
> - Pods, ReplicaSets, Services, Deployments, Secrets

In general, you can find more about objects by doing:

	kubectl get <object type> <object name>