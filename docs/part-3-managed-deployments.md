# Kubernetes 101 - Part 3: Managed deployments

In the previous part we created some objects manually using the command line.

## Levelling up

Whilst you could create files for all of your objects as Pods, Services, ReplicaSets etc. this could get a bit tedious over time. Inevitably you’ll want to reuse these files, template their values and share them with others.

For this, we’re going to use a popular tool called Helm.

> Explanation of Helm, including:
> - Charts
> - Values
> - Repository

We’re going to use Helm to deploy an application using a ‘chart’.

## Install Helm

You can install Helm using brew:

    brew install kubernetes-helm

This will install the `helm` CLI tool.

## A simple app

Let’s use Helm to install our app:

    helm install my-app ./examples/charts/my-app

You can see all the work Helm does for us: it defines Kubernetes objects like services, deployments etc. You can customise these by editing the values file and passing it to Helm when you install the chart.

> Example showing the objects created:
> Run `helm status <release name>`


## A real application

Let’s use Helm to install Jenkins:

    helm install stable/jenkins --name my-jenkins

You can see the status of your installed releases with:

    helm list

Helm generates a unique release name for every chart you install. Normally you’d set this yourself.

Let’s see what Helm just did for us. We can inspect the chart and its values using:

    helm inspect stable/jenkins
