# Deployment Strategies - Rolling Update

default of deployment strategy is rolling update

- to check deplyement startegy

```sh
kubectl describe deployment DEPLOYMENT_NAME
```

![dpstrategy](/images/deploymentstrategi.png)

- to check history of deplyment

```sh
kubectl rollout status deployment DEPLOYMENT_NAME
```

- to roll back of image version

```sh
kubectl rollout undo deployment DEPLOYMENT_NAME
```
