Install service monitor on k8s

https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html


## wait untill pod is ready

```
kubectl -n <NS> wait pod <POD>--for=condition=Ready

```

## Install metric server on eks

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
