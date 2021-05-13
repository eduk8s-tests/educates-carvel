# Eduk8s Carvel packages


## Minikube testing
You need an anonymous/public registry in minikube

### Create the package

```
cd bundles/eduk8s-operator

# Update upstream. If you want a new version, edit vendir.yml
vendir sync

# If there's changes. Update the images file
kbld -f images.yaml --imgpkg-lock-output .imgpkg/images.yml

# Push the new bundle to the registry
imgpkg push --bundle registry.minikube.failk8s.com/eduk8s/educates-operator-package:latest --file .

cd -
```

### Install the Packagerepository on your cluster

Make sure you have alpha kapp, otherwise do: 
```
kubectl apply -f https://raw.githubusercontent.com/vmware-tanzu/carvel-kapp-controller/develop/alpha-releases/v0.19.0-alpha.7.yml
```

```
cd packages

# Edit your registry name in values.yaml. Mine is registry.minikybe.failk8s.com

# Create the bundle for the packagerepository and packages. The image needs a directory packages with the packages.yaml in it.
ytt -f values.yaml -f overlay -f resources | imgpkg push -i registry.minikube.failk8s.com/eduk8s/educates-package-repository:latest -f -


# Create the package registry in the cluster
ytt -f values.yaml -f overlay -f resources/repo | kubectl apply -f -


# Create an instance of the package in the eduk8s-ctrl namespace
kubectl create ns eduk8s-ctrl
kubectl apply -f resources/installedpackages/rbac.yaml -n eduk8s-ctrl
kubectl apply -f resources/installedpackages/educates-operator.yaml -n eduk8s-ctrl

cd -
```


