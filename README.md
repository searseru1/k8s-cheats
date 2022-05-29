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