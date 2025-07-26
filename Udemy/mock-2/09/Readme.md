# Mock Exam 2 - 09

## Task: Modify Gateway

Modify the existing web-gateway on cka5673 namespace to handle HTTPS traffic on port 443 for kodekloud.com, using a TLS certificate stored in a secret named kodekloud-tls.

## Requirements:
- Is the web gateway configured to listen on the hostname kodekloud.com?
- Is the HTTPS listener configured with the correct TLS certificate?


## Documentation:
- [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/)
- [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/


## Solution Steps

1. Check existing gateway configuration:

```bash
kubectl get gateway web-gateway -n cka5673 -o yaml
```

_Result_:
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"gateway.networking.k8s.io/v1","kind":"Gateway","metadata":{"annotations":{},"name":"web-gateway","namespace":"cka5673"},"spec":{"gatewayClassName":"kodekloud","listeners":[{"name":"http","port":80,"protocol":"HTTP"}]}}
  creationTimestamp: "2025-07-26T21:34:15Z"
  generation: 1
  name: web-gateway
  namespace: cka5673
  resourceVersion: "7827"
  uid: 3fe8243a-2d23-41ad-a657-e8b2ad9bfb11
spec:
  gatewayClassName: kodekloud
  listeners:
  - allowedRoutes:
      namespaces:
        from: Same
    name: http
    port: 80
    protocol: HTTP
status:
  conditions:
  - lastTransitionTime: "1970-01-01T00:00:00Z"
    message: Waiting for controller
    reason: Pending
    status: Unknown
    type: Accepted
  - lastTransitionTime: "1970-01-01T00:00:00Z"
    message: Waiting for controller
    reason: Pending
    status: Unknown
    type: Programmed
```

2. Modify the gateway to add an HTTPS listener and use the TLS secret:
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: web-gateway
  namespace: cka5673
spec:
  gatewayClassName: kodekloud
  listeners:
  - allowedRoutes:
      namespaces:
        from: Same
    name: https
    port: 443
    protocol: HTTPS
    hostname: kodekloud.com
    tls:
      certificateRefs:
        - name: kodekloud-tls
```

_Difference:_
```yaml
  - name: https
    port: 443
    protocol: HTTPS
    hostname: kodekloud.com
    tls:
      certificateRefs:
        - name: kodekloud-tls
```

3. Apply the modified gateway configuration:

```bash
kubectl apply -f web-gateway.yaml
```

