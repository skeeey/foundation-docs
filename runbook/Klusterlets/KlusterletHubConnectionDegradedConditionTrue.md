# The `HubConnectionDegraded` condition of `klusterlet` is `True`

## Symptom

The `HubConnectionDegraded` condition of `klusterlet` on the managed cluster is `True`.

If the `HubConnectionDegraded` condition is `True`, it indicates that the Klusterlet running on the managed cluster is having trouble connecting to the hub cluster, which will cause the status of the managed cluster in the hub cluster to become `Unknown`.

## Diagnosis

1. Get the `HubConnectionDegraded` condition

```sh
oc get klusterlet klusterlet -o jsonpath='{.status.conditions[?(@.type=="HubConnectionDegraded")].status}'
```

If the status of this condition is `True`, continue the following steps

2. Check the message of the `HubConnectionDegraded` condition

```sh
oc get klusterlet klusterlet -o jsonpath='{.status.conditions[?(@.type=="HubConnectionDegraded")].message}'
```

- If the message contains "tls: failed to verify certificate: x509: certificate signed by unknown authority", this indicates that the klusterlet has an invalid hub CA, preventing it from establishing the TLS connection to the hub cluster, refers to the runbook [Configure Klusterlet CA bundle](../../guide/ConfigureKlusterletCABundle.md) to get the solution.
- If the message contains "bootstrap-hub-kubeconfig: Unauthorized", connect the ACM teams to get a solution.
