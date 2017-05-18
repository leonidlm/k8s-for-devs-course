# Useful commands & links

Examples for the service YAML format can be found here:
https://kubernetes.io/docs/concepts/services-networking/service/

```
minikube service <SERVICE>
minikube ssh
telnet <IP>:<PORT>
kubectl expose <RESOURCE> --port <> --target-port <>
```

# Instructions

## Adding Services To Wordpress And Mysql Pods

In the last exercise we've created a multi-container pod that runs wordpress
and mysql containers.

Although the containers were running perfectly, we didn't expose the services to
traffic outside our minikube cluster. Let's do it now!

1. Create a YAML service definition to route traffic to the wordpress container from the previous exercise.
2. Check that the new service has any active endpoints using the `kubectl describe svc ...` command.
2. Make sure that you can connect to the wordpress UI with a browser, use
`minikube service ...` command to connect via the newly created service.

Services are Layer 4 constructs which means you aren't restricted to HTTP only,
you can expose TCP and UDP traffic too.

1. Add another service, this time expose mysql's standard port (3306). As
before, write a small YAML definition and use the `kubectl apply` command to
submit it to the cluster.
2. Check that you can access the mysql port using `telnet`. Because minikube
runs everything in a VM, you'll need to connect to the VM first with `minikube
ssh`. Remember that you'll need to connect to the **Service IP**.

## Advanced

Exposing a pod is such a common task that kubectl has a handy command to shorten the process.

Delete the wordpress service that you have created in the first step.

Can you expose the same service using the `kubectl expose` command without using
a YAML configuration?

You can review the YAML specification that Kubernetes created for you with the
`kubectl get svc <NAME> -o yaml` command. Is it different from what you've
created yourself before?

