# Mock Exam 3 - 04

## Task: Create ConfigMap and Modify Deployment

Create a ConfigMap named app-config in the namespace cm-namespace with the following key-value pairs:

ENV=production
LOG_LEVEL=info

Then, modify the existing Deployment named cm-webapp in the same namespace to use the app-config ConfigMap by setting the environment variables ENV and LOG_LEVEL in the container from the ConfigMap.

## Requirements:

- ConfigMap app-config is created
- Deployment uses the app-config ConfigMap for variable ENV and LOG LEVEL
- Are the environment variables reflected in the deployment?
- ConfigMap has proper ENV value
- ConfigMap has proper LOG_LEVEL value

## Documentation:

- [Kubernetes ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-containers/configure-pod-configmap/)
- [Kubernetes Environment Variables](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)

## Solution Steps

Use kubectl create configmap to create the ConfigMap and kubectl set env to modify the deployment to use the ConfigMap.

Create the ConfigMap:

```bash
kubectl create configmap app-config -n cm-namespace \
  --from-literal=ENV=production \
  --from-literal=LOG_LEVEL=info
```

Patch the deployment to use the config:

```bash
kubectl set env deployment/cm-webapp -n cm-namespace --from=configmap/app-config
```
