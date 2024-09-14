# The `ManagedClusterImportSucceeded` condition of `ManagedCluster` is `False`

## Symptom

A managed cluster on the hub cluster has a condition of type `ManagedClusterImportSucceeded` with a `status` of `False`.

## Meaning

The managed cluster is failed to import into the hub cluster.

## Impact

The status of the `ManagedClusterConditionAvailable` condition will be `Unknown`.

## Diagnosis

1. Check the message of `ManagedClusterImportSucceeded` condition of the `ManagedCluster` on the hub cluster.

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterImportSucceeded")].message}'
```

If the message is about `manifestworks`, go to the step 2

2. Check the klusterlet ManifestWorks in the cluster namespace on the hub cluster.

```sh
oc -n <cluster-name> get manifestworks <cluster-name>-klusterlet -ojsonpath='{.status.conditions}'
```

If the klusterlet `ManifestWork` cannot be found, check the `ManifestWork` CRD on the hub cluster

```sh
omc get crds manifestworks.work.open-cluster-management.io -ojsonpath='{.metadata.deletionTimestamp}'
```

If the `ManifestWork` CRD is not found or it has a deletion timestamp, that means the `ManifestWork` CRD is deleted or deleting, connect the ACM teams to get a solution.

Otherwise, refers to [Import managed cluster manually](../../guide/ManagedCluster/ManagedClusterManualImport.md) as a solution to reinstall the klusterlet on the managed cluster.
