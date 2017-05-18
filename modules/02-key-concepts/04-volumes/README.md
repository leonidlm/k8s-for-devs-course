# Useful commands & links

Examples of mounting a volume:
https://kubernetes.io/docs/concepts/storage/volumes/#hostpath

Examples of a PersistentVolumeClaim and how to mount it in a Pod:
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes-1
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes

# Instructions

## Volumes

If you didn't notice it yet, but once our mysql container is terminated, all
the data is lost too.

The easiest solution to this problem is to make the mysql's `/var/lib/mysql`
data directory reside on a volume.

Modify the mysql's deployment file from lesson `03-deployments` to mount a
hostPath type volume at the `/var/lib/mysql` path inside the mysql container.

To check that your configuration is working, you can run the following command
to write some data to the directory:

```
kubectl exec -ti <MYSQL_POD_NAME> -- touch /var/lib/mysql/its_a_test
```

Then restart the mysql pod using the `kubectl delete pod <POD_NAME>` command,
and check that your data is still on disk with the next command:

```
kubectl exec -ti <NEW_MYSQL_POD_NAME> -- ls /var/lib/mysql/
```

## PersistentVolumes and Claims

To make our mysql deployment files more portable and less coupled to the
specific storage mechanism configured in the cluster, let's change the mysql
deployment to use a PersistentVolume and Claims instead of a volume.

Firstly, create a PersistentVolumeClaim, set it to 1G of storage, and submit it
to minikube's cluster.

What's its status? Can you view your claim using the `kubectl get pvc` command?
Were you required to specify the exact storage type or protocol?

Now, modify the mysql deployment from the exercise above and instead of mounting
a volume, mount the PVC directly.

Submit your new deployment file. Can you see that it's worked by running a
describe command on the newly created pod?

Did the PVC's status changed? How about the PersistentVolume that was created,
can you see it using the `kubectl get pv` command?

## Advanced

What can you tell about the storage type that was used for the
PersistentVolume by looking at the default StorageClass?
Can you see where on the host disk the PV data is saved? Try describing the PVC and PV resources.

Now, delete the mysql deployment with the `kubectl delete deployment ...` command.
What happened to the PV and PVC we have created?

Try deleting the PVC now. What happened to the PV that was created based on it?
Can we make the PersistentVolume outlive the PVC that created it?

