# Mock Exam 2 - 03

## Task: Create storage class

Create a StorageClass named rancher-sc with the following specifications:

The provisioner should be rancher.io/local-path.
The volume binding mode should be WaitForFirstConsumer.
Volume expansion should be enabled.

## Requirements:

- StorageClass rancher-sc is present
- Provisioner is rancher.io/local-path
- VolumeBindingMode is WaitForFirstConsumer

## Documentation:

- [Kubernetes Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
- [Kubernetes Local Path Provisioner](https://rancher.com/docs/local-path-provisioner/latest/)

## Solution Steps

Use kubectl create storageclass and specify the required provisioner and parameters.

Create a manifest file rancher-sc.yaml:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: rancher-sc
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
```

Apply the file on the cluster:

```bash
kubectl apply -f rancher-sc.yaml
```

To verify:

```bash
kubectl get storageclass rancher-sc -o yaml
```
