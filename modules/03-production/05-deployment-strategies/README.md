# Useful commands & links

Life-cycle hooks examples:
https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#container-hooks
https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/

Canary deployment's example:
https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#canary-deployments

Downward API's documentation:
https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/

# Instructions

## Rolling Update in bulks

By default, a rolling update deployment will release a new version of your apps
one-by-one.

If we have lots of pods running, updating each pod one-by-one can take a while.
Fortunately, we can adjust our deployment strategy parameters to speed up the
release process.

Increase the amount of replicas in the wordpress deployment file to 6.

Override the default deployment settings to make sure that you can deploy a new
version of our wordpress pods two containers at a time.

When done, try re-deploying the wordpress deployment file, do you see the
required amount of pod getting terminated in one batch?

**TIP** If for testing purposes you want to redeploy the same deployment file
 multiple times, add an annotation to `metadata.annotations` and change it's
 value with something random before each new deployment attempt.

## Canary Release

To make sure that our wordpress deployments will be safe to the end user, we
will implement a canary deployment strategy to release new versions to
production.

Copy the wordpress deployment file from the previous exercise and adjust them to
implement a canary deployment.

**TIP** you'll probably have two separate deployment files. One for the stable
version, and one for the canary release. Make sure that you use the appropriate
matchLabels and that you tag each pod with it's release version.

Now, test your new release process manually, in real-world scenarios this will
be probably automated in the CI/CD pipeline.

Deploy the stable release, let's label it `v1`. Make sure all the pods are
running and then deploy the canary release, let's label it `v2`.

Just to verify that you configured the canary release correctly, scale the v1
release pods to 0 (`kubectl scale deployment ...`) and try browsing the
wordpress UI.  If it's working it means the traffic is successfully reaching the
canary release.

What will happen if we won't set custom matchLabels directive and use the
default values specified by the deployment object? Will it affect how we'll be
able to promote the canary version to the main deployment resource?

## **Advanced**: Downward API & Hooks

Implement a simple busybox container that will monitoring and print an output of
an arbitrary logfile to it's standard output.

Use the lifecycle hooks to print the pod's IP at container's start and pod's
Namespace before it's stopped.

You can use the busybox demployment we have created in
`02-key-concepts/03-deployment` module.

