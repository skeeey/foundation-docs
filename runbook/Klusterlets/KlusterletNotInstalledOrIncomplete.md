# Klusterlet is either not installed or incomplete

## Symptom

The `klusterlet` resource cannot be found on the managed cluster, or there is no klusterlet running on a managed cluster.

## Meaning

The klusterlet is not running on the managed cluster.

## Impact

The status of this managed cluster will be `Unknown` on the hub cluster.

## Diagnosis

1. Get the `Klusterlet` resource with the following command on the managed cluster

```sh
oc get klusterlet klusterlet
```

If the command fails with timeout error, connect the User to ensure the cluster is running and reachable.

If the `klusterlet` is not found, refers to [Import managed cluster manually](../../guide/ManagedCluster/ManagedClusterManualImport.md) as a solution to reinstall the klusterlet on the managed cluster.

Otherwise, refers to the runbook [The `HubConnectionDegraded` condition of `klusterlet` is `True`](../Klusterlets/KlusterletHubConnectionDegradedConditionTrue.md) for further diagnosis. 
