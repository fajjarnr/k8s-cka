# Manage multiple Clusters with Kube Contexts

to manage multi cluster we can use context

```sh
kubectl config get-context
```

- to change another cluster

```sh
kubectl config use-context --current CONTEXT_NAME
```

- to change namespace cluster

```sh
kubectl config set-context --current --namespace=NAMESPACE_NAME
```
