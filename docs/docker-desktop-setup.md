# Kubernetes 101 - Docker Desktop Kubernetes installation

Docker Desktop ships with a local Kubernetes installation.

## Prerequisites

- Mac running macOS Sierra or later
- Homebrew
- Docker for Mac 18.12 or later

## Installation

Open the Preferences pane of Docker Desktop:

![img/dd_k8s_setup1.png]

![img/dd_k8s_setup2.png]

![img/dd_k8s_setup3.png]

It will take some time for brew to do its thing.

Once your installation is complete, let’s bring up your cluster.

## Exploring your cluster

Now you have a cluster up and running, let’s explore. We’re going to use the kubectl command line tool we installed earlier to explore the Kubernetes API.

First of all, let’s start with some basics:

	kubectl version

This shows you the client and server version. Don’t worry if they’re different - Kubernetes adheres to the Semantic Versioning standard to allow forwards and backwards compatibility between +/- 2 minor versions.

Let’s get some information about the cluster:

	kubectl cluster-info
