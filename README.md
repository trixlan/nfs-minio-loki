# Comandos para usar NFS externo

[Youtube Video]([https://](https://www.youtube.com/watch?v=6DmEp0kXUOI&t=1s))

```shell
oc create ns nfs
oc create -f rbac.yaml
Si no tiene acceso a internet se debe bajar la imagen
oc import-image nfs-subdir-external-provisioner:v4.0.2 --from=quay.io/gchavezt/nfs-subdir-external-provisioner:v4.0.2 --confirm -n nfs
oc create -f deployment.yaml
oc create -f class.yaml
oc create role use-scc-hostmount-anyuid --verb=use --resource=scc --resource-name=hostmount-anyuid -n nfs
oc get roles -n nfs
oc adm policy add-role-to-user use-scc-hostmount-anyuid -z nfs-client-provisioner --role-namespace nfs -n nfs
Scalar a 0 y regresar a 1
oc create -f test.yaml
```

# Comandos para crear MinIO

[Deploy MinIO]([https://](https://linuxelite.com.br/blog/minio/))

```shell
oc create -f minio-ns.yaml
oc create -f minio-pvc.yaml
oc create -f minio-ocp.yaml

oc get deployment/minio -o yaml | oc adm policy scc-subject-review -f -

oc get pod/minio-5cc789844f-kf8jr -o yaml | grep serviceAccountName

oc adm policy add-scc-to-user anyuid -z default

oc create -f minio-svc.yaml
Los routers se deben crear manualmente
```

# Comandos para instalar Loki

```shell
oc create -f loki-ns.yaml
oc create -f loki-operatorgroup.yaml
oc create -f loki-subscription.yaml
oc create -f logging-ns.yaml
oc create -f loki-secret.yaml
oc create -f lokistack.yaml
Verificar
oc get pods -n openshift-logging
```
