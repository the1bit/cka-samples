# Mock Exam 3 - 11

## Task: Horizontal Pod Autoscaler

Create a Horizontal Pod Autoscaler (HPA) for the deployment named api-deployment located in the api namespace.
The HPA should scale the deployment based on a custom metric named requests_per_second, targeting an average value of 1000 requests per second across all pods.
Set the minimum number of replicas to 1 and the maximum to 20.

Note: Deployment named api-deployment is available in api namespace. Ignore errors due to the metric requests_per_second not being tracked in metrics-server

## Requirements:

- Is api-hpa HPA deployed in api namespace?
- Is api-hpa configured for metric requests_per_second?

## Documentation:

- [Kubernetes Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-applications/horizontal-pod-autoscale/)
- [Kubernetes Metrics Server](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/)

## Solution Steps

Under /root/ folder you will find a yaml file api-hpa.yaml. Update the yaml file as per task given.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-hpa
  namespace: api
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-deployment
  minReplicas: 1
  maxReplicas: 20
  metrics:
    - type: Pods
      pods:
        metric:
          name: requests_per_second
        target:
          type: AverageValue
          averageValue: "1000"
```

Use below command

```bash
kubectl create -f api-hpa.yaml
```
