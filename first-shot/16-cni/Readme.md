# CKA Network Plugin Installation Example (Calico)

## Official Documentation References

- [Kubernetes Network Plugins (CNI)](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)
- [Calico Installation Guide](https://docs.projectcalico.org/getting-started/kubernetes/)
- [Calico Dual-Stack Guide](https://docs.projectcalico.org/networking/dual-stack/)
- [IPv6 Support in Kubernetes](https://kubernetes.io/docs/concepts/services-networking/dual-stack/)

---

## Simulating CNI Deletion (Calico)

To simulate CNI deletion on your kubeadm cluster, remove Calico resources:

```sh
# Delete Calico manifest and related resources
kubectl delete -f https://docs.projectcalico.org/manifests/calico.yaml

# Optionally, delete any remaining Calico CRDs
kubectl get crd | grep calico | awk '{print $1}' | xargs kubectl delete crd

# Remove Calico namespace if still present
kubectl delete ns calico-system

# Cleanup files
sudo rm -rf /etc/cni/net.d

# Reboot to ensure all CNI configurations are cleared
sudo reboot
```

**Note:**  
After deletion, pod networking will be disrupted until you reinstall a CNI plugin.


## Task: Install Calico and Enable IPv4/IPv6 (Step-by-Step)

### Step 1: Prepare Your Cluster for Dual-Stack

1. **Verify Kubernetes Version**
   - Ensure your cluster is v1.20+ (dual-stack support is beta from v1.20).
2. **Check kubeadm config**
   - By default, `/etc/kubernetes/kubeadm-config.yaml` does not exist after `kubeadm init` unless you created it manually.
   - To enable dual-stack, create a config file (e.g., `kubeadm-config.yaml`) with both IPv4 and IPv6 CIDRs:
     ```yaml
     apiVersion: kubeadm.k8s.io/v1beta3
     kind: ClusterConfiguration
     networking:
       podSubnet: "192.168.0.0/16,fd00:1::/64"
     ```

    - Enable IP forwarding for both IPv4 and IPv6 on all nodes:
     ```sh
     sudo sysctl -w net.ipv4.ip_forward=1
     sudo sysctl -w net.ipv6.conf.all.forwarding=1
     ```
   - Then re-initialize your cluster with:
     ```sh
     kubeadm reset
     sudo kubeadm init --config /etc/kubernetes/kubeadm-config.yaml
     # Or use your custom path, e.g., --config ./kubeadm-config.yaml
     # Note: The config file can be named and placed anywhere; just provide the correct path.
     ```
   - This ensures your cluster supports dual-stack networking.

### Step 2: Download and Edit Calico Manifest

1. **Download the manifest:**
   ```sh
   curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/calico.yaml

   ```
2. **Edit for dual-stack:**
   - Open `calico.yaml` and locate the `calico-node` DaemonSet environment variables.
   - Add or update:
     ```yaml
     ---
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: calico-node
      namespace: kube-system
    spec:
      template:
        spec:
          containers:
            - name: calico-node
            # ...other config...
            env:
              # Add your variables below, among the other env entries:
              - name: CALICO_IPV4POOL_CIDR
                value: "192.168.0.0/16"
              - name: CALICO_IPV6POOL_CIDR
                value: "fd00:1::/64"
              - name: IP_AUTODETECTION_METHOD
                value: "can-reach=8.8.8.8"
              # ...existing env variables...
     ```

   - Ensure `calico_backend` is set to `bird` in the `calico-config` ConfigMap.

### Step 3: Apply Calico Manifest

```sh
kubectl apply -f calico.yaml
```

### Step 4: Verify Installation

1. **Check Calico pods:**
   ```sh
   kubectl get pods -n kube-system -l k8s-app=calico-node
   ```
2. **Check node IPs:**
   ```sh
   kubectl get nodes -o wide
   ```
3. **Check pod networking:**
   - Deploy a test pod and check it gets both IPv4 and IPv6 addresses.

### Step 5: Troubleshooting

- If pods do not get IPv6 addresses, check node network config and Calico logs:
  ```sh
  kubectl logs -n kube-system -l k8s-app=calico-node
  ```
- Ensure your cluster and nodes support IPv6 routing.

### Step 6: Remove the taint from the master node

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

## Test network connectivity

```sh
# Create a test pod
kubectl apply -f pod.yaml
# Check pod IPs
kubectl get pods -o wide
# Test connectivity
kubectl exec -it dualstack-test -- apt update
kubectl exec -it dualstack-test -- apt install -y iputils-ping
kubectl exec -it dualstack-test -- ping6 fd00:1::1
```


**References:**
- [Calico Dual-Stack Guide](https://docs.projectcalico.org/networking/dual-stack/)
- [Kubernetes Dual-Stack](https://kubernetes.io/docs/concepts/services-networking/dual-stack/)

## Exam Tips

- Always use the latest manifest from the official documentation.
- For dual-stack, verify your cluster supports IPv4/IPv6 before applying Calico.
- Use `kubectl get pods -n kube-system` to confirm Calico pods are running.

---

## Example Commands

```sh
# Install Calico
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# Verify pods
kubectl get pods -n kube-system

# Edit ConfigMap for dual-stack (if needed)
kubectl edit configmap calico-config -n kube-system
```

---

## Summary Table

| Plugin  | IPv4 | IPv6 | Official Docs |
|---------|------|------|--------------|
| Calico  | Yes  | Yes  | [Calico](https://docs.projectcalico.org/getting-started/kubernetes/) |

---

**Practice:**  
Install Calico in a test cluster, enable dual-stack, and verify pod networking.

