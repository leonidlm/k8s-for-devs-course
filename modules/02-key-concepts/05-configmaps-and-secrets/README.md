# Useful commands & links

Example of mounting a configmap as a volume:
https://github.com/kubernetes/kubernetes.github.io/blob/master/docs/user-guide/configmap/mount-file-pod.yaml

Example of exposing a secret's value as environment variable:
https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables

```
kubectl edit <CONFIG_MAP>
Kubectl create configmap <NAME> --from-literal=<KEY>=<VALUE>
kubectl create configmap <NAME> --from-file=<FILENAME>
# Default key is the name of the file, to override:
--from-file=<KEY>=<PATH_TO_FILE>

echo "<DECODED_SECRET_DATA>" | base64 -d

# Mapping a secret to Environment variable:
env:
  - name: ENV_VAR_NAME
    valueFrom:
      secretKeyRef:
        name: secret-name
        key: secret-key

```

# Instructions

## ConfigMaps

We can mount a ConfigMap as a file in any Pod's filesystem.

For this exercise, we'll take the busybox pod we have created previously, and
make it present a welcome message we'll define in a separate ConfigMap.

Start by creating a new ConfigMap. You can name it anyway you like.
Add a simple text message to the content of one of the keys. You can use the
YAML file format or the `kubectl create configmap <NAME> --from-file=...` command.

Now, adjust the pod YAML file from lesson 01-pods to inject the content of the
ConfigMap you just created in a `/opt/app/message` file inside the running container.

You can adjust the container's command to the following so that it will print
the content of the file:

```
command:
- /bin/sh
- -c
- "cat /opt/app/message && sleep 1000"
```

Deploy the new pod and view it's logs with `kubectl logs <POD>` command.

## Secrets

Until now, we saved the mysql's root password inside the wordpress deployment file.
It would be better to save the password in Kubernetes and reference it in the
deployment file instead.

1. Create a new secret in Kubernetes. Try using both the YAML definition and the
`kubectl create secret generic <NAME> --from-literal ...` command.
2. Modify the wordpress deployment to get the mysql password from a secret. The
simplest way of doing so is by mapping a secret to an environment variable.
Remember that you should also deploy the mysql deployment and service object for
the wordpress pods to succesfully connect to their database.
3. Try changing the secret's value by redeploying it's configuration. Check if
the value of the environment variable in the pod also changed (use `kubectl exec
<POD> env` command).
4. Can you edit the secret's values using the `kubectl edit` command? Do you see
the password's value as you defined it before?
5. If you describe one of the wordpress pods, can you see the root's password
in the output?

