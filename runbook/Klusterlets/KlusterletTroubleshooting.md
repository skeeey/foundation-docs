# Troubleshooting the Klusterlet on the managed cluster

## Diagnosis

Check the `Klusterlet` resource with the following command on the managed cluster

```sh
oc get klusterlet klusterlet
```

If the `klusterlet` is not found, refers to [Import managed cluster manually](../../guide/ImportManagedClusterManually.md) as a solution to reinstall the klusterlet on the managed cluster.

Otherwise, refers to the runbook [The `HubConnectionDegraded` condition of `klusterlet` is `True`](../Klusterlets/KlusterletHubConnectionDegradedConditionTrue.md) for further diagnosis. 
