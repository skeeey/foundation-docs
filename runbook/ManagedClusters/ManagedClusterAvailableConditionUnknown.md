# The `ManagedClusterConditionAvailable` condition of `ManagedCluster` is `Unknown`

## Symptom

A managed cluster on the hub cluster has a condition of type `ManagedClusterConditionAvailable` with a `status` of `Unknown`.

If the `ManagedClusterConditionAvailable` condition is `Unknown`, it indicates that the Klusterlet stops to update its lease in the hub cluster, which will cause the status of the managed cluster in the hub cluster to become `Unknown` and the status of the `Available` condition of all `ManagedClusterAddOns` for this managed cluster to become `Unknown` as well.

## Diagnosis

1. Check the status of `ManagedClusterConditionAvailable` condition of your cluster on the hub cluster.

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterConditionAvailable")].status}'
```

If the status is `Unknown`, continue the following steps.

2. Check the status of `ManagedClusterImportSucceeded` conditions of your cluster on the hub cluster.

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterImportSucceeded")].status}'
```

If the `ManagedClusterImportSucceeded` condition is `False`, Refers to the runbook [The `ManagedClusterImportSucceeded` condition of `ManagedCluster` is `False`](./ManagedClusterImportSucceededConditionFalse.md) for further diagnosis.

Otherwise, Refers to the runbook [Troubleshooting the Klusterlet on the managed cluster](../Klusterlets/KlusterletTroubleshooting.md) for further diagnosis.
