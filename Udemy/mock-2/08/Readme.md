# Mock Exam 2 - 08


## Task: HPA

Create a Horizontal Pod Autoscaler with name backend-hpa for the deployment named backend-deployment in the backend namespace with the webapp-hpa.yaml file located under the root folder.
Ensure that the HPA scales the deployment based on memory utilization, maintaining an average memory usage of 65% across all pods.
Configure the HPA with a minimum of 3 replicas and a maximum of 15.


## Requirements:
- Is backend-hpa HPA deployed in backend namespace?
- Is deployment configured for metrics memory utilization?

## Documentation:
- [Kubernetes Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-applications/horizontal-pod-autoscale/)
- [Kubernetes Metrics Server](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/)  


## Solution Steps

1. Edit a file named `webapp-hpa.yaml`:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: webapp-hpa
  namespace: backend
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: kkapp-deploy
  minReplicas: 3
  maxReplicas: 15

```

2. Expand the spec section to include the following:

```yaml
  metrics:
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 65
```

_Full file content should look like this:_

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
  namespace: backend
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend-deployment
  minReplicas: 3
  maxReplicas: 15
  metrics:
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 65
```

3. Apply the configuration using the command:

```bash
kubectl apply -f webapp-hpa.yaml
```

4. Verify the HPA is created and configured correctly:

```bash
kubectl get hpa -n backend
```

5. Check the details of the HPA to ensure it is monitoring memory utilization:

```bash
kubectl describe hpa webapp-hpa -n backend
```
