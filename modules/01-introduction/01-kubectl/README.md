# Prerequisites

1. Install Docker
2. Install Kubectl
3. Install Minikube

# Useful commands & links

```
minikube start
minikube status
minikube delete

kubectl create -f <YAML_FILE>
kubectl get nodes
kubectl get pods
kubectl get services
kubectl describe service <NAME>
kubectl describe pod <NAME>
kubectl logs <POD_NAME>
```

# Instructions

1. Start a new Kubernetes cluster using `minikube`
2. Make sure that your cluster is running
3. Check that your `kubectl` is connected to the new cluster. You can verify it by checking the node names in the cluster (`kubectl get nodes`)
4. Upload the configuration defined in `hello-world.yml` file (`kubectl create -f <file-name>`)
5. Wait until the application's pod will be in the **running** state by querying it's status with
`kubectl get pods` command
    1. If it's not running yet, can you check why? Use the describe command
    mentioned below and look at the **Events** field.
6. View the application's pod definition by using the `kubectl describe pod
<POD_NAME>` command
    1. Can you discover pod's internal IP address?
    2. How about it's node. Can you find the IP of the node that is running this pod?
7. Use `minikube service hello-world` to open the application in the browser
8. Now let's stop the application by running `kubectl delete pod <NAME>`
9. Can you access it's UI now? Are there any listed endpoints for the applications service? (`kubectl describe service hello-world`)

