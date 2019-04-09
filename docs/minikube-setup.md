# Kubernetes 101 - Minikube installation

Minikube is a local distribution of Kubernetes.

## Prerequisites

- Mac running macOS Sierra or later
- Homebrew
- Docker for Mac 17.06 or later
- VirtualBox

## Installation

We’re going to use Homebrew to install the Minikube package:

	brew cask install minikube

It will take some time for brew to do its thing.

Once your installation is complete, let’s bring up your cluster.

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
