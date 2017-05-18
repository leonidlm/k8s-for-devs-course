# Useful commands & links

Life-cycle hooks examples:
https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#container-hooks
https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/

Canary deployment's example:
https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#canary-deployments

Downward API's documentation:
https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/

# Instructions

## Deployment Strategy

To make sure that our wordpress deployments will be safe to the end user, we
will implement a canary deployment stategy to release new versions to
production.

Copy the wordpress deployment file from the previous exercise and adjust them to
implement a canary deployment.

**TIP** you'll probably have two separate deployment files. One for the stable
version, and one for the canary release.  Make sure that you use the appropriate
matchLabels and that you tag each pod with it's release version.

Now, test your new release process manually, in real-world scenarios this will
be probably automated in the CI/CD pipeline.

Deploy the stable release, let's label it `v1`. Make sure all the pods are
running and then deploy the canary release, let's label it `v2`.

Just to verify that you configured the canary release correctly, scale the v1
release pods to 0 (`kubectl scale deployment ...`) and try browsing the
wordpress UI.  If it's working it means the traffic is successfully reaching the
canary release.

## **Advanced**: Downward API & Hooks

Implement a simple busybox container that will monitoring and print an output of
an arbitraty logfile to it's standard output.

Use the lifecycle hooks to print the pod's IP at container's start and pod's
Namespace before it's stopped.

You can use the busybox demployment we have created in
`02-key-concepts/03-deployment` module.

