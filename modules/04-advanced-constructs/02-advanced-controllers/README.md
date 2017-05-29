# Useful commands & links

Examples of CronJob resources:
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

A good DaemonSet resource example:
https://github.com/kubernetes/kubernetes/blob/master/examples/newrelic/newrelic-daemonset.yaml

StatefulSet resource examples and documentations:
https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/

# Instructions

## 1

We would like to run a simple cron task that will check an availability of
ynet.co.il website once every minute.

As before, we can use a simple busybox container and run a following wget
command to check if ynet's website is up and running:

```
image: busybox
args:
  - /bin/bash
  - -c
  - wget http://www.ynet.co.il -O/dev/null
```

We also want to make sure that we won't re-try an unsuccessful attempt, we can
just wait for the next time the check should be run.

Pick one of the advanced resource we discussed in this module and implement this
periodic task using it.

Is there a command that can show up the results of the last 10 checks?

## 2

We need to implement a simple cpu load monitoring agent for Kubernetes.
It's main task is collect and print the cpu load on every Kubernetes node that
is available in the cluster.

As before, we can use the busybox container with a few changes.

To expose the cpu metrics of the node to the container that runs in a pod, we
will need to mount the host's `/proc/loadavg` file inside the container.

Assuming that we mounted host's `/proc/loadavg` inside a container at
`/host/proc/loadavg`, we can continuously print the load avarage by running the
busybox container with the following arguments:
```
args:
  - /bin/sh
  - -c
  - while true; do cat /host/proc/loadavg ; sleep 5; done
```

Which resource from the resources we discussed in this module would fit best to
do this job? Remember that we need to run this task on **every node in the
cluster**

## 3

We need to run 3 containers that should have predictable DNS names. Let's say
**q-1.queue.default**, **q-2.queue.default** and **q-3.queue.default**.

We can emulate a simple HTTP echo service using the busybox container and the following arguments:

```
args:
  - /bin/sh
  - -c
  - nc -l 0.0.0.0 0 -p 80
```

Pick the best resource from what we have learned to deploy this containers in
K8s.

**TIP** When you creating a service to expose these pods inside the cluster,
set the `spec.ClusterIP` to `None`.

Did you notice something strange in the way the pods started for the first time?

You can also check that you configured the DNS name resolution correctly by
running the following command (which will run a one-off ping command from one
pod in the cluster to another):

```
kubectl exec -ti q-0 -- ping -c 1 q-1.queue.default
```

