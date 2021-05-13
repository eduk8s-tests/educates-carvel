# Create the bundle repository



## Package a bundle


## Update bundle repository

```
imgpkg push -i quay.io/eduk8s/educates-package-repository:latest -f packages
```


## Deploy the bundle repository to your cluster

```
kubectl apply -f repo/educates-packagerepository.yaml
```


## Install the operator

```
kubectl apply -f installedpackages/rbac.yaml
kubectl apply -f installedpackages/educates-operator.yaml
```