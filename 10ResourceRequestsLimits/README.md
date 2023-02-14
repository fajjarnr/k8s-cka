# Resource Requests & Limits

we can add request and limit on resource deployment

```sh
resources:
 requests:
  memory: "500Mi"
  cpu: "200m"
 limits:
  memory: "1Gi"
  cpu: "500m"
```

- to check resources, we can use jsonpath

```sh
kubectl get pod -o jsonpath="{range .items[*]} {.metadata.name}{.spec.containers[*].resources}{'\n'}"
```
