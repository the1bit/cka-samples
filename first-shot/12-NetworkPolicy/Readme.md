# Kubernetes NetworkPolicy Example: Enable Communication Between Frontend and Backend in Different Namespaces

## Official Documentation Reference

- [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [NetworkPolicy API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#networkpolicy-v1-networking-k8s-io)

## Scenario

- **Goal:** Allow communication from frontend (`fe`) pods in namespace `frontend` to backend (`be`) pods in namespace `backend`.
- **Requirement:** No permissive (i.e., do not allow all traffic).

## Step-by-Step Solution

### 1. Create Namespaces

```bash
kubectl create namespace fe
kubectl create namespace be
```

### 2. Create Example Frontend and Backend Apps

```bash
kubectl run fe-app --image=busybox --restart=Never --namespace=fe --command -- sleep 3600
kubectl run be-app --image=nginx --namespace=be

```

### 3. Label the Pods and Namespaces

```bash
kubectl label pod fe-app app=frontend -n fe
kubectl label pod be-app app=backend -n be
kubectl label namespace fe name=fe
kubectl label namespace be name=be
```

### 4. Apply the NetworkPolicy

```bash
kubectl apply -f allow-fe-to-be.yaml
```

### 5. Test the Policy

```bash
kubectl get pod be-app -n be -o wide
kubectl exec -n fe fe-app -- wget -O- <BE_APP_POD_IP>:80
```

- Try to connect from a `fe` pod to a `be` pod (should succeed).
- Try to connect from any other pod (should fail).

## Key Points for CKA

- Use `namespaceSelector` and `podSelector` for fine-grained control.
- Always label namespaces and pods as required by your policy.
- Avoid using empty selectors (which are permissive).
- Test your policy with `kubectl exec` or similar tools.

## Useful Links

- [NetworkPolicy Examples](https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-examples)
- [CKA Exam Tips](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

**Practice:**  
Try to write and apply a similar policy for egress, or for other namespace combinations.
