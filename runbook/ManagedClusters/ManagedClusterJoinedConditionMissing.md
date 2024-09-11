# The `ManagedClusterJoined` condition of `ManagedCluster` is not present

## Symptom

A managed cluster on the hub cluster has no condition of type `ManagedClusterJoined`. 

## Meaning
The registration of this managed cluster is not finished successfully.

## Impact

Once this issue happens, the status of the condition `ManagedClusterConditionAvailable` for this managed cluster will become `Unknown` eventually.

## Diagnosis

1. Run the following command to check the condition `ManagedClusterJoined`

```sh
oc get managedcluster <cluster-name> -o jsonpath='{.status.conditions[?(@.type=="ManagedClusterJoined")]}'
```

If the command does not have output, continue the following steps.

2. Refers to the runbooks below for the diagnosis instructions if any condition matches.
- [The `ManagedClusterImportSucceeded` condition of `ManagedCluster` is `False`](./ManagedClusterImportSucceededConditionFalse.md)

3. On the managed cluster, refers to the runbooks below for the diagnosis instructions if any condition matches

- [Klusterlet is either not installed or incomplete](../Klusterlets/KlusterletNotInstalledOrIncomplete.md)
- [The `HubConnectionDegraded` condition of `klusterlet` is `True`](../Klusterlets/KlusterletHubConnectionDegradedConditionTrue.md)
