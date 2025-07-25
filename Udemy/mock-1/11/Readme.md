# Mock Exam 1 - 11

## Task: Create a Kubernetes Gateway

Create a Kubernetes Gateway resource with the following specifications:
- Name: web-gateway
- Namespace: nginx-gateway
- Gateway Class Name: nginx
- Listeners:
  - Protocol: HTTP
  - Port: 80
  - Name: http

## Requirements:
- Is the web-gateway deployed to listen on port 80?

## Documentation:
- [Kubernetes Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway/)


## Solution Steps

1. **Check gateway class availability**

   Ensure that the Gateway API is installed in your cluster. You can check for existing GatewayClasses with:

```sh
   kubectl get gatewayclasses
```

2. **Create the Gateway resource**

Create a file named `web-gateway.yaml` with the following content:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: web-gateway
  namespace: nginx-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    protocol: HTTP
    port: 80
```

3. **Apply the Gateway resource:**

   Use the following command to create the Gateway in your cluster:

```sh
   kubectl apply -f web-gateway.yaml
```


4. **Verify the Gateway:**

   Check if the Gateway has been created successfully and is listening on port 80:

```sh
   kubectl get gateway web-gateway -n nginx-gateway
   kubectl describe gateway web-gateway -n nginx-gateway
```