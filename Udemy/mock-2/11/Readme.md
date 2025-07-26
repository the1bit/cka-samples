# Mock Exam 2 - 11

## Task: NetworkPolicy

You are requested to create a NetworkPolicy to allow traffic from frontend apps located in the frontend namespace, to backend apps located in the backend namespace, but not from the databases in the databases namespace. There are three policies available in the /root folder. Apply the most restrictive policy from the provided YAML files to achieve the desired result. Do not delete any existing policies.

## Requirements:
- Correct NetworkPolicy applied
- Incorrect NetworkPolicy is not applied
- Second incorrect NetworkPolicy is not applied

## Documentation:
- [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [Kubernetes Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

## Solution Steps

1. Check existing Files in the /root folder:

```bash
ls -l net-pol-*
```

_Result_:
```bash
-rw-r--r-- 1 root root 268 Jul 26 21:56 net-pol-1.yaml
-rw-r--r-- 1 root root 339 Jul 26 21:56 net-pol-2.yaml
-rw-r--r-- 1 root root 267 Jul 26 21:56 net-pol-3.yaml
```

2. Review the content of each NetworkPolicy file:

```bash
cat net-pol-1.yaml
```

_Result_:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: net-policy-1
  namespace: backend
spec:
  podSelector: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          access: allowed
    ports:
    - protocol: TCP
      port: 80
```

```bash
cat net-pol-2.yaml
```

_Result_:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: net-policy-1
  namespace: backend
spec:
  podSelector: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          access: allowed
    ports:
    - protocol: TCP
      port: 80

controlplane ~ ➜  cat net-pol-2.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: net-policy-2
  namespace: backend
spec:
  podSelector: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: frontend
    - namespaceSelector:
        matchLabels:
          name: databases
    ports:
    - protocol: TCP
      port: 80
```

```bash
cat net-pol-3.yaml
```

_Result_:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: net-policy-3
  namespace: backend
spec:
  podSelector: {}
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: frontend
    ports:
    - protocol: TCP
      port: 80
  ```


3. Understand the differences:
net-pol-1.yaml: Too broad; allows traffic from any namespace with a certain label.
net-pol-2.yaml: Incorrect; explicitly allows both frontend and databases.
net-pol-3.yaml: Correct; only allows traffic from the frontend namespace.

3. Apply the most restrictive NetworkPolicy:

```bash
kubectl apply -f net-pol-3.yaml
```


4. Verify the applied NetworkPolicy:

```bash
kubectl get networkpolicy -n backend
```


