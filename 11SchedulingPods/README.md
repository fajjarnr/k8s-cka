# Scheduling Pods

you can schedule pod with nodeName or nodeSelector

- nodeName

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: worker1
```

but if we have random name (cloud provider) of node we can add label on node

```sh
kubectl get node

kubectl label node NODENAME type=memory

kubectl get nodes --show-labels
```

- then create yaml file with nodeSelector and key value of label

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-2
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    type: memory
```

- then we can see

```sh
kubectl get pod nginx-2 -o wide
```

![nodepodlabel](/images/nodepodlabel.png)

### with nodeSelector is not flexible and to do this we can use nodeAffinity

on node affinity have 2 option

- required -> hard
- preferred -> soft

```yaml
requiredDuringSchedulingIgnoredDuringExecution
preferredDuringSchedulingIgnoredDuringExecution
```

- after apply pod yaml file we can check

```sh
kubectl get pod PODNAME -o wide
kubectl get pod -o wide
```

![nodeAffinity](/images/nodeAffinity.png)
