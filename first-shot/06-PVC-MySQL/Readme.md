# Recreate a Deleted PVC for MySQL and Rebind It

## Scenario

- A Deployment or Pod is using a PersistentVolumeClaim named mysql-pvc
- The PVC was accidentally deleted
- Your task:
    - Recreate the PVC
    - Ensure it binds to the original PersistentVolume (PV), if it still exists
    - Make sure the MySQL pod starts successfully with the volume reattached


## How I want to do:
1. Create a PVC and PV
2. Deploy MySQL to it
3. Delete PVC
4. Create a new PVC
5. Configure MySQL deployment to use the new PVC


## Commands

```bash
# Create namespace for CKA MySQL
kubectl apply -f 01-namespace.yaml

# Create PersistentVolume
kubectl apply -f 02-pv.yaml

# Create PersistentVolumeClaim
kubectl apply -f 03-pvc.yaml

# Deploy MySQL
kubectl apply -f 04-mysql-deployment.yaml

# Check the status of the MySQL pod
kubectl get pods -n cka-mysql
```

Reproduce issue:
```bash
# Delete the PVC
kubectl delete pvc mysql-delete-pvc -n cka-pv

# Check the status of the MySQL pod
kubectl get pods -n cka-mysql
```

```bash
# Create a new PVC
kubectl apply -f 05-new-pvc.yaml

# Check the status of the new PVC
kubectl get pvc -n cka-pv

# Patch the MySQL deployment to use the new PVC
kubectl patch deployment mysql -n cka-pv \
  --type=json \
  -p='[{"op": "replace", "path": "/spec/template/spec/volumes/0/persistentVolumeClaim/claimName", "value": "mysql-pvc"}]'

# Restart the MySQL deployment to apply the new PVC
kubectl rollout restart deployment mysql -n cka-pv

# Check the status of the MySQL pod again
kubectl get pods -n cka-mysql
```

