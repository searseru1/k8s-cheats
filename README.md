# k8s-cheats

###  Stop container
```
      containers:
        - args:
            - while true; do echo 'sleep' && sleep 5; done;
          command:
            - sh
            - '-c'
```

### Ingress annotation
```
kubernetes.io/ingress.class: nginx
nginx.ingress.kubernetes.io/whitelist-source-range: cidr
```
### Cert managet annotation
```
cert-manager.io/cluster-issuer: letsencrypt-prod
```
### Cert manager issuer example
```
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    email: youremail@something.com
    privateKeySecretRef:
      name: production-issuer-account-key
    server: https://acme-v02.api.letsencrypt.org/directory
    solvers:
    - http01:
        ingress:
          class: nginx
EOF
```


### Azure ZRS blockdisk
```
cat <<EOF | kubectl apply -f -
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azuredisk-standart-zrs
provisioner: disk.csi.azure.com
parameters:
  skuname: StandardSSD_ZRS
allowVolumeExpansion: true
reclaimPolicy: Delete
volumeBindingMode: Immediate
EOF
```
### Overrides
```
ingress:
  enabled: true
  annotations:
     kubernetes.io/ingress.class: nginx
     nginx.ingress.kubernetes.io/whitelist-source-range: cidr
     cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: domain.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: secret-tls
      hosts:
        - domain.com
```
### Cli
```
kubectl -n monitor exec -it $(kubectl -n monitor get pods -l app.kubernetes.io/name=fluentdloki --output=jsonpath={.items..metadata.name}) bash

curl -X POST -d 'json={"src":"http"}' http://localhost:8881

curl -i -H "Content-type: application/json" -X POST --data '{ "streams": [ { "labels": "{source=\"JSON\",job=\"simpleJsonJob\", host=\"SimpleHost\"}", "entries": [{ "ts": "2022-05-31T16:48:20Z", "line": "TEST!" }] } ] }' http://localhost:8881
```