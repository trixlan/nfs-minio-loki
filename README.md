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
oc project minio-ocp
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

# Comandos para instalar Logging

```shell
oc create -f logging-operatorgroup.yaml
oc create -f logging-subscription.yaml
oc create sa logging-collector -n openshift-logging
oc adm policy add-cluster-role-to-user logging-collector-logs-writer -z logging-collector -n openshift-logging
oc adm policy add-cluster-role-to-user collect-application-logs -z logging-collector -n openshift-logging
oc adm policy add-cluster-role-to-user collect-infrastructure-logs -z logging-collector -n openshift-logging
oc create -f logging-clusterlogforwarder.yaml

Verificar
oc get pods -n openshift-logging

oc apply -f UIPlugin.yaml
```

# Revisar NFS

sudo yum install -y nfs-utils
   10  mkdir /var/nfsshare
   11  sudo mkdir /var/nfsshare
   12  sudo chmod 777 /var/nfsshare/
   13  sudo chmod 777 /var/nfsshare
   14  ls -la /var/
   15  cat > /etc/exports << EOF
   16  /var/nfsshare     *(rw,sync,no_wdelay,no_root_squash,insecure,fsid=0)
   17  EOF
   18  sudo cat > /etc/exports << EOF
   19  /var/nfsshare     *(rw,sync,no_wdelay,no_root_squash,insecure,fsid=0)
   20  EOF
   21  sudo vi /etc/exports
   22  cat /etc/exports
   23  systemctl start rpcbind
   24  sudo systemctl start rpcbind
   25  sudo systemctl start nfs-server
   26  sudo systemctl start nfs-lock
   27  sudo systemctl start nfs-idmap
   28  cat /etc/os-release 
   29  cat /etc/exports
   30  sudo exportfs -a
   31  sudo systemctl enable rpcbind
   32  sudo systemctl enable nfs-server
   33  export FIREWALLD_DEFAULT_ZONE=`firewall-cmd --get-default-zone`
   34  sudo mkdir -p /nfs_share/data
   35  sudo chmod -R 777 /nfs_share/data/
   36  sudo vi /etc/exports
   37  sudo exportfs -a
   38  sudo systemctl status nfs-server
   39  setenforce --help
   40  setenforce 0
   41  sudo setenforce 0
   42  sudo getenforce
   43  rpcinfo -p
   44  cd /var/nfsshare/
   45  ls
   46  cd nfs-mariadb-pvc-ee023874-e9c1-439a-b8b7-11490fc7348f/
   47  ls
   48  ls -la
   49  cd ibdata1 
   50  ls
   51  cd ..
   52  ls
   53  cd nfs-test-nfs-provisioner-pvc-e54c926e-ff1e-4dce-b400-a9b7ac57f083/
   54  ls
   55  exit
   56  cd /var/nfsshare/
   57  ls
   58  cd nfs-test-nfs-provisioner-pvc-e54c926e-ff1e-4dce-b400-a9b7ac57f083/
   59  ls
   60  cd
   61  exit
   62  history

