# The `ManagedClusterImportSucceeded` condition of `ManagedCluster` is `False`

## Symptom

A managed cluster on the hub cluster has a condition of type `ManagedClusterImportSucceeded` with a `status` of `False`.

If the  `ManagedClusterImportSucceeded` condition is `False`, it indicates that the managed cluster failed to import into the hub cluster, which will cause the status of managed cluster in the hub cluster to become `Unknown`.

## Diagnosis

1. Check the message of `ManagedClusterImportSucceeded` condition of the `ManagedCluster` on the hub cluster.

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterImportSucceeded")].message}'
```

If the message is about `manifestworks`, go to the step 2

2. Check the ManifestWorks of the managed cluster in the cluster namespace on the hub cluster.

```sh
oc -n <cluster-name> get manifestworks <cluster-name>-klusterlet -ojsonpath='{.status.conditions}'
```

If the `ManifestWork` cannot be found, check the `ManifestWork` CRD on the hub cluster

```sh
oc get crds manifestworks.work.open-cluster-management.io -ojsonpath='{.metadata.deletionTimestamp}'
```

If the `ManifestWork` CRD is not found or it has a deletion timestamp, that means the `ManifestWork` CRD is deleted or deleting, which means the ACM hub control plane is broken. Once this occurs, you need reinstall the ACM hub control plane and reimport your cluster to resolve this problem.
