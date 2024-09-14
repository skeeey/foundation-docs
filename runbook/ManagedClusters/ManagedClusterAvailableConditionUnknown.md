# The `ManagedClusterConditionAvailable` condition of `ManagedCluster` is `Unknown`

## Symptom

A managed cluster on the hub cluster has a condition of type `ManagedClusterConditionAvailable` with a `status` of `Unknown`.

## Meaning

When the klusterlet agent starts running on the managed cluster, it updates a `Lease` resource in the cluster namespace on the hub cluster every N seconds, where N is configured in the `spec.leaseDurationSeconds` of the `ManagedCluster`. If the `Lease` resource is not updated in the past 5 * N seconds, the status of the `ManagedClusterConditionAvailable` condition for this managed cluster will be set to `Unknown`. Once the klusterlet agent connects back to the hub cluster and continues to update the `Lease` resource, the ManagedCluster will become available automatically.

## Impact

Once this issue happens

- When getting a managed cluster from the ACM console or command, the managed cluster status will be `Unknown`;
- Usually both the klusterlet agent and add-on agents cannot connect to the hub cluster. Changes on ManifestWorks and other add-on specific resources on the hub cluster can not be pulled to the managed cluster;
- The status of the `Available` condition of all `ManagedClusterAddOns` for this managed cluster will be set to `Unknown` as well;
- The status of the `ManifestWorks` on the hub cluster is stale. You can not trust it anymore;

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

Otherwise, continue the following steps

3. Refers to the runbooks [Klusterlet is either not installed or incomplete](../Klusterlets/KlusterletNotInstalledOrIncomplete.md) for further diagnosis.
