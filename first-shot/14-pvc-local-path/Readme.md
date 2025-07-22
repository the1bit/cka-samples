# PersistentVolumeClaim with rancher.io/local-path

## Official Documentation References

- [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)
- [Local Path Provisioner (rancher.io/local-path)](https://github.com/rancher/local-path-provisioner)

## Key Concepts

- **PersistentVolume (PV):** Cluster resource representing storage.
- **PersistentVolumeClaim (PVC):** Request for storage by a user.
- **StorageClass:** Defines how storage is dynamically provisioned.
- **local-path:** A StorageClass that provisions PVs using local disk paths.

## Example: Create a PVC using rancher.io/local-path

### 1. Check for local-path StorageClass

```sh
kubectl get storageclass
```

Look for `local-path`. If not present, install [local-path provisioner](https://github.com/rancher/local-path-provisioner#installation).

```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.31/deploy/local-path-storage.yaml
```

### 2. Create a PersistentVolumeClaim

```sh
kubectl apply -f 01-pvc.yaml
```

### 3. Verify PVC Status

```sh
kubectl -n cka-local-path get pvc local-path-pvc
```

```bash
# WaitForFirstConsumer 
kubectl -n cka-local-path describe pvc local-path-pvc
```


Status should be `Bound`.

### 4. Use PVC in a Pod

```sh
kubectl apply -f 02-app.yaml
```

## Exam Tips

- Know how to create and use PVCs.
- Understand StorageClass and dynamic provisioning.
- Practice with `local-path` for local storage scenarios.

## References

- [Kubernetes Storage Concepts](https://kubernetes.io/docs/concepts/storage/)
- [Local Path Provisioner](https://github.com/rancher/local-path-provisioner)
