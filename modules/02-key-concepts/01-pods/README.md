# Useful commands & links

Pod YAML examples:
https://github.com/kubernetes/contrib/blob/master/apparmor/loader/example-pod.yaml

Another example:

```
apiVersion: v1
kind: Pod
metadata:
  name: example
spec:
  restartPolicy: Always
  containers:
    - name: example
      image: "repo/image:tag"
      args:
        - run
        - --export
        - something
      env:
        - name: SERVER_PROTOCOL
          value: tcp
```

# Instructions

## Simple Echo Pod

We'll get our hands dirty by creating and running a simple pod from scratch.

The simplest pod we can run is a `busybox` container. To simulate a long
running process, we'll specify a sleep command that will make the container run
indefinitely.

Create a Kubernetes YAML definition for an equivalent of the following
Docker run command:

```
docker run -ti busybox:latest sleep 1000
```

This is the part of the YAML that specifies the sleep command, make sure to fill
in the rest of the YAML:

```
containers:
  - args:
    - sleep
    - "1000"
```

## Multi-Container Pods

For this exercise we would like to run a wordpress and a mysql containers in one
pod.

We'll use the official docker images, here are a few examples for how you would
run them in your local machine:

```
docker run -p 8080:80 wordpress
docker run -p 3306:3306 mysql:5.5
```

Create a Kubernetes YAML definition of a pod that will run:

1. One `wordpress` container
2. One `mysql` container (version *5.5*)
3. Remember, since both of these containers run in a pod, they can communicate via localhost.
4. Connect the `wordpress` container to it's local `mysql` by specifying the
following environment variables in the pod's YAML definition:
`WORDPRESS_DB_PASSWORD` (should be `root`) and `WORDPRESS_DB_HOST`.
5. For the `mysql` container, specify the following environment variable: `MYSQL_ROOT_PASSWORD` (should be `root`).

Now, after you made sure that everything is running properly, try killing one of
the containers in a pod (use `minikube shh` and `docker stop`).

Does the pod status changes? Is the killed container gets resurrected?

## Advanced

Try using the `kubectl port-forward` command to access the wordpress UI

