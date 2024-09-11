# Failed to verify certificate: x509: certificate signed by unknown authority

## Symptom

The `HubConnectionDegraded` condition of klusterlet is `True`, and the condition message contains "tls: failed to verify certificate: x509: certificate signed by unknown authority"

## Meaning

When the klusterlet connects to the hub cluster, it uses TLS to establish the connection. If this issue occurs, it indicates that the klusterlet has an invalid hub CA, preventing it from establishing the TLS connection to the hub cluster.

## Impact

The klusterlet can not access the hub cluster, the status of the condition `ManagedClusterConditionAvailable` for the managed cluster will become `Unknown` eventually.

## Solution

1. Download and encode the root CA certificate `ISRG Root X1`;

```sh
curl -s https://letsencrypt.org/certs/isrgrootx1.pem | base64 | tr -d "\n"
```

2. Create a KlusterletConfig resource with this encoded CA certificate

```sh
oc apply -f - <<EOF
apiVersion: config.open-cluster-management.io/v1alpha1
kind: KlusterletConfig
metadata:
  name: isrg-root-x1-ca
spec:
  hubKubeAPIServerCABundle: "<output from step 1>"
EOF
```

3. Annotate the managed cluster;

```sh
oc annotate --overwrite managedcluster local-cluster agent.open-cluster-management.io/klusterlet-config='isrg-root-x1-ca'
```

4. If the state of the local-cluster does not recover in 1 minute, export the import.yaml and apply it.

```sh
oc get secret local-cluster-import -n local-cluster -o jsonpath={.data.import .yaml} | base64 --decode > import.yaml
oc apply -f import.yaml
```

**Note**: For any new cluster, the annotation `agent.open-cluster-management.io/klusterlet-config: isrg-root-x1-ca` should be added into the ManagedCluster resource.
