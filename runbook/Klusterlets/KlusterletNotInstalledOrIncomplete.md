# Klusterlet is either not installed or incomplete

## Symptom

The `Klusterlet` resource cannot be found on the managed cluster, or there is no klusterlet running on a managed cluster.

## Diagnosis

On the managed cluster

1. Check the `Klusterlet` resource with the following command

```sh
oc get klusterlet klusterlet
```

If the `Klusterlet` is not found, follow the instructions from [Import managed cluster manually](../../guide/ManagedCluster/ManagedClusterManualImport.md) to reinstall the klusterlet on the managed cluster.

2. Check the klusterlet operator pod with the following command

```sh
oc -n open-cluster-management-agent get pod -l app=klusterlet
```

If the pod is not found, follow the instructions from [Import managed cluster manually](../../guide/ManagedCluster/ManagedClusterManualImport.md) to reinstall the klusterlet on the managed cluster.
