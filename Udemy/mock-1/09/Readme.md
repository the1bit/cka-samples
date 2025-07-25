# Mock Exam 1 - 09

## Task: Create Horizontal Pod Autoscaler

Create a Horizontal Pod Autoscaler (HPA) with name webapp-hpa for the deployment named kkapp-deploy in the default namespace with the webapp-hpa.yaml file located under the root folder.
Ensure that the HPA scales the deployment based on CPU utilization, maintaining an average CPU usage of 50% accross all pods. Configure the HPA to cautiously scale down pods by setting a stability window of 300 seconds to prevent rapid fluctuations in pod count.

Note: The kkapp-deploy deployment is created for backend; you can check in the terminal.


## Requirements:
- Is the HPA webapp-hpa deployed?
- Is the deployment configured for metrics CPU Utilization?
- Is the stabilization window set to 300 seconds?

## Solution Steps

1. **Check webapp-hpa.yaml file**

   ```sh
   cat /root/webapp-hpa.yaml

   ```yaml
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: webapp-hpa
    namespace: default
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: kkapp-deploy
     minReplicas: 2
     maxReplicas: 10
   ```


2. **Expand the spec section to include CPU utilization:**

```yaml
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
   ```


3. **Add stabilization window:**

```yaml
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
```


4. **Apply the HPA:**
   ```sh
   kubectl apply -f /root/webapp-hpa.yaml
   ```

5. **Verify the HPA:**
   ```sh
    kubectl get hpa webapp-hpa
    kubectl describe hpa webapp-hpa
    ```

6. **Check the deployment:**
   ```sh
   kubectl get deployment kkapp-deploy
   kubectl describe deployment kkapp-deploy
   ```  

