# kubernetes cloud-native operator installation

1. Create a dynatrace namespace
```
kubectl create namespace dynatrace
```

2. Install Dynatrace Operator
```
kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v1.3.0/kubernetes.yaml
kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v1.3.0/kubernetes-csi.yaml
```
- Run the following command to see when Dynatrace Operator components finish initialization:
```
kubectl -n dynatrace wait pod --for=condition=ready --selector=app.kubernetes.io/name=dynatrace-operator,app.kubernetes.io/component=webhook --timeout=300s
```

3. Create secret for Access tokens
Create a secret named dynakube for the Dynatrace Operator token and data ingest token obtained in Tokens and permissions required.
```
kubectl -n dynatrace create secret generic dynakube --from-literal="apiToken=<OPERATOR_TOKEN>" --from-literal="dataIngestToken=<DATA_INGEST_TOKEN>"
```

4. Apply the DynaKube custom resource

Download the DynaKube custom resource sample for cloud native full stack from GitHub﻿. In addition, you can review the available parameters or how-to-guides, and adapt the DynaKube custom resource according to your requirements.

Run the command below to apply the DynaKube custom resource, making sure to replace <your-DynaKube-CR> with your actual DynaKube custom resource file name. A validation webhook will provide helpful error messages if there's a problem.
```
kubectl apply -f <your-DynaKube-CR>.yaml
```

5. (optional) Verify deployment
Verify that your DynaKube is running and all pods in your Dynatrace namespace are running and ready.
```
$ kubectl get dynakube -n dynatrace
NAME         APIURL                                          STATUS     AGE
dynakube     https://<ENVIRONMENTID>.live.dynatrace.com/api  Running    45s
```
