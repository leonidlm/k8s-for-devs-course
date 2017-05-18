# Useful commands & links

Deployment YAML examples can be found here:
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

```
kubectl delete <pod>
kubectl scale <RESOURCE> <NAME> --replicas=<NUM>
```

# Instructions

## Simple Deployment

Let's take the busybox pod from a few exercises before and turn it into a
deployable object.

Take the busybox's pod definition and translate it into a deployment.
Can you run at least 3 replicas of this pod with one deployment?

After you've create and submitted the deployment file, check that all the
replicas are up and running with the `kubectl get deployment <NAME>` command.

Now, let's change something and release a new version of our app.

Change the sleep time to 500, save the deployment file and apply the changes.

What happens now?
Do you see more pods of our busybox container than is defined in the `replicas` field?
What is the current revision of our deployment object? (`kubectl rollout history ...`)

## Splitting The Pod Into Separate Deployments

In real-world examples we won't run wordpress and mysql containers in one pod.

Usually, you will have separate deploymet object for each one of these applications.

Let's split the pod definition from a few exercises before into separate deployment files.

1. Create a deployment YAML file for the mysql container and upload it to Kubernetes.
2. Create a deployment YAML file for the wordpress container. Make sure that you
run at least 3 replicas of the wordpress pods. Also, make sure the wordpress
containers can connect to the mysql pod. Use the mysql service's default DNS to
point the wordpress containers to the correct pods. When done, deploy the wordpress
YAML file too.
3. Create services for both mysql and wordpress pods (or use `kubectl expose`
to make wordpress pods accessible outside the cluster and mysql pods accessible
internally)
4. View the logs of the mysql and wordpress pods. Is everything up and running
as expected? You can try accessing the wordpress UI with the
`minikube service <NAME>` command.
5. Kill one of the pods, is it resurrected?
6. If you delete the deployment object of one of the apps, what happens?

## Advanced

1. Create a headless service for the wordpress deployment. What changed? Can you
still access it's UI?
2. What happens if you delete the ReplicaSet created by the wordpress deployment?
3. Can you scale down the number of wordpress pods to one?

