# 9. Migrated Cluster Failed: Check and Fix

## Official Documentation References
- [Troubleshooting Clusters](https://kubernetes.io/docs/tasks/debug/debug-cluster/)
- [Control Plane Node Troubleshooting](https://kubernetes.io/docs/tasks/debug/debug-cluster/control-plane-node/)
- [Node Troubleshooting](https://kubernetes.io/docs/tasks/debug/debug-cluster/node/)
- [Pod Troubleshooting](https://kubernetes.io/docs/tasks/debug/debug-cluster/pod/)
- [Cluster Administration](https://kubernetes.io/docs/tasks/administer-cluster/cluster-management/)

## Step-by-Step CKA Approach

### 1. Check Cluster Status
```sh
kubectl get nodes
kubectl get pods --all-namespaces
kubectl get componentstatuses
```

### 2. Investigate Control Plane Components
- Check kube-apiserver, kube-controller-manager, kube-scheduler logs:
```sh
sudo journalctl -u kubelet
docker ps | grep kube-apiserver
docker logs <container-id>
```
- Ensure manifests exist in `/etc/kubernetes/manifests/`

### 3. Node Issues
- Check node status:
```sh
kubectl describe node <node-name>
kubectl get events --all-namespaces
```
- Verify kubelet is running:
```sh
systemctl status kubelet
```

### 4. Pod Issues
- Check pod status and logs:
```sh
kubectl get pods -A
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace>
```

### 5. Networking Issues
- Check CNI plugin status:
```sh
kubectl get pods -n kube-system
kubectl describe pod <cni-pod> -n kube-system
```

### 6. Common Fixes
- Restart failed components:
```sh
systemctl restart kubelet
```
- Fix misconfigured manifests in `/etc/kubernetes/manifests/`
- Ensure certificates and kubeconfig files are valid

## Exam Tips
- Always start with `kubectl get nodes` and `kubectl get pods -A`
- Use `kubectl describe` and `kubectl logs` for details
- Check official docs for troubleshooting steps

## Useful Links
- [Kubernetes Troubleshooting Guide](https://kubernetes.io/docs/tasks/debug/)
- [Cluster Management](https://kubernetes.io/docs/tasks/administer-cluster/cluster-management/)
