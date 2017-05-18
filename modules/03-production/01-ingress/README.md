# Useful commands & links

Examples of ingress resource specification:
https://kubernetes.io/docs/concepts/services-networking/ingress/#simple-fanout

# Instructions

We are going to create a simple drinks api.

Our api will support two drinks at the moment: pepsi and cola.
When a user will hit `http://<api-endpoint>/<drink-name>` he will be presented
with a short message about the drink.

For example, let's say our api is accessible at `www.cooldrinks.com`, we want
that when a user browses to `www.cooldrinks.com/pepsi` he'll get a short message
provided by the **pepsi** micro-service.

## Step 1: Create the backend services

Create two nginx based micro-services, one called *pepsi* the second *cola*. A
micro-service should consist of a deployment and a service (NodePort).

I've placed an example nginx pod and an nginx configuration in the `examples` directory.

To create a pepsi mico-service for example, you'll need to:

1. Create a configmap from the example nginx's configuration
2. Create a deployment based on the nginx pod example
3. Make sure to mount the nginx confgimap to `/etc/nginx/conf.d/<NAME>` inside the pod.

## Step 2: Creat the ingress resource

Now, after you created the two micro-services and verified that they work by
hitting their service IPs, it's time to work on the ingress definitions.

Create an ingress resource the will route the url requests to the relevant services.
A request to `http://<INGRESS_IP>/pepsi` should be proxied to the pepsi service you just wrote.
And a request to `http://<INGRESS_IP>/cola` should be proxied to the cola service.

Requests to `http://<INGRESS_IP>/` or any other unsupported URL should return
an 404 error.

## Step 3: Change the routing

Now, let's change how our application should be accessed.
We want to route the traffic based on a subdomain instead of a URL path.
This means, if a user browses to `pepsi.cooldrinks.com` we want to present the
`pepsi` service response. Same logic should apply to the `cola.cooldrinks.com`.

Adjust your ingress resource to support this routing.

Before you can test your changes, add the following two lines to your
`/etc/hosts` file:

```
<INGRESS_IP>   pepsi.cooldrinks.com
<INGRESS_IP>   cola.cooldrinks.com
```

