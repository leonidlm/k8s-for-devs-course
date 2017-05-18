# Useful commands & links

The YAML definition of a new namespace:

```
---
apiVersion: v1
kind: Namespace
metadata:
  name: my-new-namespace
```

```
kubectl get pod -Lapp
kubectl get pod --show-labels

kubectl config set-context minikube --namespace=kube-system
kubectl --namespace kube-system get pods
kubectl get pods --all-namespaces
```

# Instructions

## Namespaces

In this exercise we will experiment with deploying resources into multiple K8s
namespaces.

Create two new namespaces named `frontend` and `backend`.

Deploy the wordpress pods into the frontend namespaces and mysql pod into the
backend namespace.

What should be changed in the wordpress deployment file to make sure the pods
can communicate with the mysql database?

Do you need to change anything in the wordpress and mysql service files?

