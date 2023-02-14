# 5 - Control Access with Users & Permissions

## RBAC

### CREATE USER ACCOUNT

- generate rsa key with openssl

```sh
openssl genrsa -out dev-tom.key 2048
```

![openssl](/images/openssl_key.png)

- request certificate name from rsa key for user tom and output csr file

```sh
openssl req -new -key dev-tom.key -subj "/CN=tom" -out dev-tom.csr
```

- see csr decode file

```sh
cat dev-tom.csr | base64 | tr -d "\n"
```

![catcsr](/images/catcsr.png)

- apply csr for user tom

```sh
kubectl apply -f dev-tom-csr.yml
```

- get certficatesigningrequest

```sh
kubectl get csr
```

![getcsr](/images/getcsr.png)

- approve certificate for user tom

```sh
kubectl certificate approve dev-tom
```

![getcsrapprove](/images/getcsrapprove.png)

- print csr tom with yaml format

```sh
kubectl get csr dev-tom -o yaml
```

![getcsrtom](/images/getcrttom.png)

- decode certificate and save to crt file

```sh
echo "CERTIFICATE" | base64 -d > dev-tom.crt
```

![cstoutput](/images/crtoutput.png)

---

### CONNECT TO CLUSTER WITH USER

- copy kubeconfig to current path

```sh
cp ~/.kube/config .
```

- decode crt file

```sh
base64 dev-tom.crt | tr -d "\n"
```

![cstoutput](/images/getcrt.png)

- decode rsa key

```sh
base64 dev-tom.key | tr -d "\n"
```

![cstoutput](/images/getkey.png)

- edit kube config file user, copy decode crt & key file and paste to config file on client-certificate-data & client-key-data

![cstoutput](/images/tomconfig.png)

- test get pod on cluster

```sh
kubectl --kubeconfig config get pod
```

- or we can override kubeconfig to .kube folder

```sh
mv config ~/.kube/
```

- check pod with user tom kubeconfig

```sh
kubectl get pod
```

![cstoutput](/images/tesgetpoduser.png)

---

### CREATE PERMISSION

- create cluster role

```sh
kubectl create clusterrole dev-cr --verb=get,list,create,update,delete --resource=deployments.apps,pods --dry-run=client -o yaml > dev-cr.yml
```

- apply cluster role with admin config

```sh
kubectl apply --kubeconfig config -f dev-cr.yml
```

- get cluster role with admin config

```sh
kubectl --kubeconfig config get clusterrole
```

![cstoutput](/images/getcr.png)

- create cluster role binding with user tom

```sh
kubectl create clusterrolebinding dev-crb --clusterrole=dev-cr --user=tom --dry-run=client -o yaml > dev-crb.yml
```

- describe cluster role binding with admin config

```sh
kubectl describe --kubeconfig config clusterrolebinding dev-crb
```

![cstoutput](/images/describecrb.png)

- check pod with user tom

```sh
kubectl get pod

kubectl auth --kubeconfig config can-i create deployment --as tom
kubectl auth --kubeconfig config can-i create pod --as tom
kubectl auth --kubeconfig config can-i get nodes --as tom
kubectl auth --kubeconfig config can-i create service --as tom
kubectl auth --kubeconfig config can-i delete deployment --as tom
```

![cstoutput](/images/testusertom.png)

---

### CREATE SERVICE ACCOUNT

- create service account for jenkins

```sh
kubectl create serviceaccount jenkins --dry-run=client -o yaml > jenkins-sa.yaml
```

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: null
  name: jenkins
```

- describe jenkins service account

```sh
kubectl describe serviceaccount jenkins
```

![cstoutput](/images/sawoutsecret.png)

- we can get token with create token

```sh
kubectl create token jenkins  --duration 10m
```

- or with create secret using yaml file

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-name
  annotations:
    kubernetes.io/service-account.name: serviceaccount-name
type: kubernetes.io/service-account-token
```

```sh
kubectl describe serviceaccount jenkins
```

![serviceaccount-with-secrete](/images/sawsecret.png)

- get secret jenkins token from jenkins service account

```sh
kubectl get secret jenkins-token -o yaml
```

![describe-secret](/images/desibesecret.png)

- decode token

```sh
echo "TOKEN" | base64 --decode
```

- create role for jenkins cicd

```sh
kubectl create role cicd-role --verb=create,list,update --resource=deployments.apps,services --dry-run=client -o yaml > cicd-role.yaml
```

- create role binding for jenkins cicd role

```sh
kubectl create rolebinding cicd-binding --role=cicd-role --serviceaccount=default:jenkins --dry-run=client -o yaml > cicd-binding.yaml
```

- apply jenkins role and role binding

```sh
kubectl apply -f cicd-role.yaml
kubectl apply -f cicd-binding.yaml
```

- check with kubectl auth

```sh
kubectl auth can-i create deployment --as system:serviceaccount:default:jenkins -n default
kubectl auth can-i create service --as system:serviceaccount:default:jenkins -n default
kubectl auth can-i get pod --as system:serviceaccount:default:jenkins -n default
```

![jenkins-role](/images/jenkinsrole.png)
