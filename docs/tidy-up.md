# Kubernetes training: Tidy up

This section helps you tidy up the objects created through this course.

You can skip items if you did not cover those sections, but the order here is useful.

## Kubernetes 101

### Helm

Delete the objects created by the Helm chart:

    helm delete my-app

### Other Kubernetes objects

Delete the service:

    kubectl delete service my-app

Delete the deployment:

    kubectl delete deployment my-app

Delete the pod:

    kubectl delete pod my-app

## Kubernetes 201

### Knative

Delete the Knative service object:

    kubectl delete kservice my-app

Uninstall Knative:

    kubectl delete -f https://github.com/knative/net-istio/releases/download/v0.22.0/net-istio.yaml
    kubectl delete -f https://github.com/knative/serving/releases/download/v0.22.0/serving-core.yaml
    kubectl delete -f https://github.com/knative/serving/releases/download/v0.22.0/serving-crds.yaml

### Istio

Delete the Bookinfo application:

    kubectl delete virtualservice reviews
    kubectl delete -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/networking/bookinfo-gateway.yaml
    kubectl delete -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/networking/destination-rule-all.yaml
    kubectl delete -f https://raw.githubusercontent.com/istio/istio/release-1.10/samples/bookinfo/platform/kube/bookinfo.yaml

Uninstall Istio:

    istioctl x uninstall --purge

Turn off Istio's sidecar injection:

    kubectl label namespace default istio-injection-

> Note the dash at the end; this removes the label.
