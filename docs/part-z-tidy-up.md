# Kubernetes 101 - Part Z: Tidy up

This section helps you tidy up the objects created through this course.

You can skip items if you did not cover those sections, but the order here is useful.

## Helm cleanup

Delete the objects created by the Helm chart:

    helm delete my-app

## Other Kubernetes objects

Delete the service:

    kubectl delete service my-app

Delete the deployment:

    kubectl delete deployment my-app

Delete the pod:

    kubectl delete pod my-app
