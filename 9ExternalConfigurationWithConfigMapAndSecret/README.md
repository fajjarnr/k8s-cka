- example to make secret key for db username

```sh
 echo -n "admin" | base64
```

- example to make secret key for db password

```sh
 echo -n "rahasia" | base64
```

- to apply all yaml file in folder

```sh
kubectl apply -f folder/
kubectl apply -f .
```

- describe secret

```sh
kubectl describe secret myapp-secret
```

- to see secret file or configmap file on deployment

```sh
kubectl logs myapp
```

![logsprintenv](/images/printenvlogssecret.png)
![logsprintenv](/images/printlogscfsecrete.png)

- to make new secret integrate with deployment we must restart pod with rollout

```sh
kubectl rollout restart deployment/my-db
kubectl rollout status deployment/my-db
```
