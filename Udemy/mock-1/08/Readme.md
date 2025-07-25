# Mock Exam 1 - 08

## Task: Create Persistent Volume

Create a Persistent Volume with the given specification:
- Vloume name: pv-analytics
- Storage: 100Mi
- Access mode: ReadWriteMany
- HostPath: /pv/data-analytics


Requirements:
- Is the volume name set?
- Is the storage capacity set?
- Is the access mode set?
- Is the hostPath set?


## Solution Steps

1. **Create the Persistent Volume YAML file**

   Create a file named `pv-analytics.yaml` with the following content:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/data-analytics
```

2. **Apply the Persistent Volume:**
   ```sh
   kubectl apply -f pv-analytics.yaml
   ```