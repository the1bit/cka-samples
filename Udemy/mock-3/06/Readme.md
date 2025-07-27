# Mock Exam 3 - 06

## Task: Create NetworkPolicy

We have deployed a new pod called np-test-1 and a service called np-test-service. Incoming connections to this service are not working. Troubleshoot and fix it.
Create NetworkPolicy, by the name ingress-to-nptest that allows incoming connections to the service over port 80.

Important: Don't delete any current objects deployed.
Important: Don't Alter Existing Objects!

## Requirements:

- NetworkPolicy: Is it applied to all sources (Incoming traffic from all pods)?
- NetworkPolicy: Is the port correct?
- NetworkPolicy: Is it applied to the correct Pod?

## Documentation:

- [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [Kubernetes Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

## Solution Steps

Solution manifest file to create a network policy ingress-to-nptest as follows:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
    - Ingress
  ingress:
    - ports:
        - protocol: TCP
          port: 80
```
