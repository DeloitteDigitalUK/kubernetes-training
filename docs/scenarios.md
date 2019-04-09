

## Pods evicted due to disk pressure

This happens when your node runs out of disk space.

Pods will have a status of 'Evicted' and its events indicate the reason:

```bash
$ kc describe pod qa-jenkins-548b54dfd7-28b9c

Events:
  Type     Reason                 Age               From                                               Message
  ----     ------                 ----              ----                                               -------
  Warning  EvictionThresholdMet   11m               kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Attempting to reclaim nodefs
  Normal   NodeHasDiskPressure    11m               kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Node ip-10-45-0-90.eu-west-1.compute.internal status is now: NodeHasDiskPressure
  Normal   NodeHasNoDiskPressure  6m (x3 over 12h)  kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Node ip-10-45-0-90.eu-west-1.compute.internal status is now: NodeHasNoDiskPressure

```

We can see the node, `ip-10-45-0-90.eu-west-1.compute.internal` ran low on disk space.

Let's have a look at the node:

```bash
$ kubectl describe node ip-10-45-0-90.eu-west-1.compute.internal

Events:
  Type     Reason                 Age               From                                               Message
  ----     ------                 ----              ----                                               -------
  Warning  EvictionThresholdMet   11m               kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Attempting to reclaim nodefs
  Normal   NodeHasDiskPressure    11m               kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Node ip-10-45-0-90.eu-west-1.compute.internal status is now: NodeHasDiskPressure
  Normal   NodeHasNoDiskPressure  6m (x3 over 12h)  kubelet, ip-10-45-0-90.eu-west-1.compute.internal  Node ip-10-45-0-90.eu-west-1.compute.internal status is now: NodeHasNoDiskPressure

```


