# Useful commands & links

Examples of ingress resource specification:
https://kubernetes.io/docs/concepts/services-networking/ingress/#simple-fanout

```
kubectl get ingress
kubectl describe ingress <INGRESS_NAME>
```

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
micro-service should consist of a deployment and a service.

I've placed both pepsi/cola nginx configuration and pod files in the `examples` directory.

To create a pepsi mico-service for example, you'll need to:

1. Upload pepsi's nginx config file to Kubernetes as a ConfigMap object
(`kubectl create configmap pepsi-nginx --from-file=default.conf=pepsi-nginx.conf`).
2. Deploy the pod as-is or create a deployment file based on the pepsi-pod
example. Make sure to mount the nginx confgimap to `/etc/nginx/conf.d/<NAME>` inside the pod.
3. Create a service to route traffic to the pepsi pod's port. Do we must to use
a NodePort service or it's not required?

## Step 2: Create the ingress resource

Now, after you created the two micro-services and verified that they work by
browsing to their service IPs, it's time to work on the ingress definitions.

Create an ingress resource that will route the url requests to the relevant services.
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

