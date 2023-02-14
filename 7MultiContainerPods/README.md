# Multi Container Pods

```sh
kubectl get pod
```

![get-status-pod](/images/initstatus.png)

```sh
kubectl logs nginx-deployment-7cb4c95d96-vq6np -c log-sidecar
kubectl logs nginx-deployment-7cb4c95d96-vq6np -c mydb-available
```

```sh
kubectl create service clusterip mydb-service  --tcp=80:80
```
