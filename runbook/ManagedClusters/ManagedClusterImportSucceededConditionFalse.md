# The `ManagedClusterImportSucceeded` condition of `ManagedCluster` is `False`

## Symptom

A managed cluster on the hub cluster has a condition of type `ManagedClusterImportSucceeded` with a `status` of `False`.

## Meaning

The managed cluster is failed to import.

## Impact

Once this issue happens, the status of the `ManagedClusterConditionAvailable` condition will be `Unknown`.

## Diagnosis

1. Check the status of `ManagedClusterImportSucceeded` condition of the `ManagedCluster` on the hub cluster.

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterImportSucceeded")].status}'
```

If the status of this condition is `False`, follow the instructions from [Import managed cluster manually](../../guide/ManagedCluster/ManagedClusterManualImport.md) to reinstall the klusterlet on the managed cluster.
