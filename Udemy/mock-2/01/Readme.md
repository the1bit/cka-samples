# Mock Exam 2 - 01

## Task: Create a Storage Class

Create a StirageClass named local-sc with the following specifications and set it as the default StorageClass:
- Provisiononer should be `kubernetes.io/no-provisioner`
- The volumeBindingMode should be `WaitForFirstConsumer`
- Volume expansion should be enabled


## Requirements:
- Is the local-sc StorageClass created
- Is provisioner set to `kubernetes.io/no-provisioner`
- Is the volumeBindingMode set to `WaitForFirstConsumer`
- Is the StorageClass set as default

## Documentation:
- [Kubernetes Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes)


## Solution Steps

1. **Create the StorageClass resource**

   Create a file named `local-sc.yaml` with the following content:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-sc
  annotations:
    storageclass.kubernetes.io/is-default-class: "true" # Set this StorageClass as default
provisioner: kubernetes.io/no-provisioner # indicates that this StorageClass does not support automatic provisioning
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true # Enable volume expansion
``` 

2. **Apply the StorageClass configuration**

   Use the following command to apply the configuration:

```bash
kubectl apply -f local-sc.yaml
```

3. **Verify the StorageClass**

   Check if the StorageClass is created and set as default by running:
```bash
kubectl get storageclass
```

