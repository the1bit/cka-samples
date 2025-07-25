# Mock Exam 1 - 10

## Task: Deploy a VerticalPodAutoscaler

Deploy a Vertical Pod Autoscaler (VPA) with name analytics-vpa for the deployment named analytics-deployment in the default namespace. The VPA should automatically adjust the CPU and memory requests of the pods to optimize resource utilization. Ensure that the VPA operates in Auto mode, allowing it to evict and recreate pods with updated resource requests as needed.

## Requirements:
- Is the VPA analytics-vpa created for deployment analytics-deployment?
- Is the VPA operating in Auto mode for deployment analytics-deployment?

## Related documentation:
- [Vertical Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/vertical-pod-autoscaler/)

## Solution Steps

1. **Create the VPA YAML file**

   Create a file named `analytics-vpa.yaml` with the following content:

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: analytics-vpa
  namespace: default
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: analytics-deployment
  updatePolicy:
    updateMode: Auto
``` 

2. **Apply the VPA resource:**

   Use the following command to create the VPA in your cluster:

```sh
    kubectl apply -f analytics-vpa.yaml
```

3. **Verify the VPA:**

   Check if the VPA has been created successfully and is operating in Auto mode:

```sh
   kubectl get vpa analytics-vpa -n default
```

   Ensure that the `updatePolicy` section shows `updateMode: Auto` and that it is targeting the `analytics-deployment`. 