# Prerequisites

1. Install Docker
2. Install Kubectl
3. Install Minikube

# Useful commands

```
minikube start
minikube status
kubectl create -f <file.yaml>
kubectl get pods
kubectl get services
kubectl describe service <service-name>
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

# Instructions

1. Start a new Kubernetes cluster using `minikube`
2. Make sure that your cluster is running
3. Check that your `kubectl` is connected to the new cluster. You can verify it by checking the node names in the cluster (`kubectl get nodes`)
3. Upload the configuration defined in `hello-world.yml` file (`kubectl create -f <file-name>`)
4. Wait until the application's pod will be in the **running** state by querying it's status with
`kubectl get pods` command
5. View the application's pod definition by using the `kubectl describe pod
<pod-name>` command
    1. Can you discover pod's internal ip address?
    2. How about it's node. Can you find on which node it's running?
7. Use `minikube service hello-world` to open the application in the browser
8. Now let's stop the application by running `kubectl delete pod <pod-name>`
9. Can you access it's UI now? Are ther any listed endpoints for the applications service? (`kubectl describe service hello-world`)

