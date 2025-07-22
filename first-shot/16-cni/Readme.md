# CKA Network Plugin Installation Example (Calico)

## Official Documentation References

- [Kubernetes Network Plugins (CNI)](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)
- [Calico Installation Guide](https://docs.projectcalico.org/getting-started/kubernetes/)
- [Calico Dual-Stack Guide](https://docs.projectcalico.org/networking/dual-stack/)
- [IPv6 Support in Kubernetes](https://kubernetes.io/docs/concepts/services-networking/dual-stack/)

---

## Task: Install Calico and Enable IPv4/IPv6

### 1. Install Calico

Calico supports both IPv4 and IPv6.  
**Official Quick Install:**

```sh
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

---

### 2. Enable IPv6 (Dual-Stack)

To enable dual-stack (IPv4/IPv6), you must:

- Ensure your Kubernetes cluster is configured for dual-stack networking ([Kubernetes Dual-Stack](https://kubernetes.io/docs/concepts/services-networking/dual-stack/)).
- Edit the Calico manifest (`calico.yaml`) or patch the `calico-config` ConfigMap after installation.

**Example ConfigMap changes:**

```yaml
# In calico-config ConfigMap:
calico_backend: "bird"
ipam: "calico-ipam"
# Enable dual-stack
CALICO_IPV4POOL_CIDR: "192.168.0.0/16"
CALICO_IPV6POOL_CIDR: "fd00:1::/112"
```

**Reference:**  
[Calico Dual-Stack Guide](https://docs.projectcalico.org/networking/dual-stack)

---

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

