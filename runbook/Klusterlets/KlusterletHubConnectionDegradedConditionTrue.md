# The `HubConnectionDegraded` condition of `klusterlet` is `True`

## Symptom

The `HubConnectionDegraded` condition of `klusterlet` on the managed cluster is `True`.

## Meaning

The klusterlet running on the managed cluster has trouble when it tries to access the hub cluster cluster.

## Impact
The klusterlet can not access the hub cluster. Once this issue happens, the status of the condition `ManagedClusterConditionAvailable` for this managed cluster will become `Unknown` eventually.

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

- If the message contains "tls: failed to verify certificate: x509: certificate signed by unknown authority", refers to the runbook [Failed to verify certificate: x509: certificate signed by unknown authority](../Certificates/x509:CertificateSignedByUnknownAuthority.md) to get the solution.
- If the message contains "bootstrap-hub-kubeconfig: Unauthorized", connect the ACM teams to find a solution.
