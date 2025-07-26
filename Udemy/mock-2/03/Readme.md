# Mock Exam 2 - 03

## Task: Create Ingress for depoloyment

A Deplyoment named webapp-deploy is running in the ingress-ns namespace and is exposed via Service named webapp-svc.

Create an ingress resource called webapp-ingress in the same namespace that will route  traffic to the service. The ingress must:
- Use pathType: Prefix
- Route request sent to path / to the backend service.
- Forward trafic to port 80 of the service.
- Be configured for the host kodekloud-ingress.app.

Test app availability using the following command:

```bash
curl -s http://kodekloud-ingress.app
```

## Requirements:
- Ingress exposed and serving traffic via the kodekloud-ingress.app host.


## Documentation:
- [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Kubernetes Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)

## Solution steps:

1. Create a file named `ingress.yaml` with the following content:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: webapp-ingress
  namespace: ingress-ns
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: kodekloud-ingress.app
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: webapp-svc
            port:
              number: 80
```

2. Apply the configuration using the command:

```bash
kubectl apply -f ingress.yaml
```

3. Verify the ingress is working by running:

```bash
curl -s http://kodekloud-ingress.app
```
