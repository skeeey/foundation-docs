# Configure Klusterlet CA bundle

1. Download and encode the root CA certificate `ISRG Root X1`;
```sh
curl -s https://letsencrypt.org/certs/isrgrootx1.pem | base64 | tr -d "\n"
```

2. Create a KlusterletConfig resource with this encoded CA certificate on the hub cluster

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

3. If your ACM version is less than 2.11, annotate the managed clusters
```sh
oc annotate --overwrite managedcluster <cluster_name> agent.open-cluster-management.io/klusterlet-config='isrg-root-x1-ca'
```

**Note**: For any new cluster of the ACM hub cluster, the above annotation should be added into the ManagedCluster.

4. The state of some managed clusters might recover soon. For those clusters that are still in unknown state, export the import.yaml from the hub cluster.
```sh
oc get secret <cluster_name>-import -n <cluster_name> -o jsonpath='{.data\.import\.yaml}' | base64 --decode > <cluster_name>-import.yaml
```

And then apply it on the managed cluster.
```sh
oc apply -f <cluster_name>-import.yaml
```