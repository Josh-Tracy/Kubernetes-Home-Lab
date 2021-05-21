# Grafana Application Deployment for Home Lab


## Persistent Volume NFS Mounting
### Create an NFS Export
On my nfs server, Ill create a /grafana to be used with the persistent volume
- Note: The export needs to have the owner and group 472, or the container will failto build due to permissiosn

### PVC For NFS
```
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: grafana-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
      #storageClassName: nfs-storage-class
```
### PV For NFS
```
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: grafana-pv
spec:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 192.168.50.113
    path: "/grafana"
```

