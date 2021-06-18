# The Data Plane

![Istio architecture](img/istio-arch.svg)

> Source: https://istio.io/latest/docs/ops/deployment/architecture/

## Installing Istio

### Prerequisites

**IMPORTANT** You must increase your memory allocation in your local cluster for Istio, or it will not run properly.

	Docker menu > Preferences > Resources
	
	Set CPUs: 4 (6 preferred)
	Set Memory: 5GB (6 preferred)

### Installation

Let's download the Istio bits:

	mkdir istio && cd istio
	curl -L https://istio.io/downloadIstio | sh -

This fetches the latest stable version.

Let's install it with the demo profile:

	cd istio-1.9.5
	istioctl install --set profile=demo -y

Check it's up and running:

	kubectl -n istio-system get deploy
	kubectl -n istio-system get IstioOperator installed-state -o yaml
