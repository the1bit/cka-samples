# Mock Exam 1 - 07

## Task: Expose hr-web-app

Expose the hr-web-app created in the previous task using a service named hr-web-app-service. The service should accessible on port 30082 on the nodes of the cluster.

The web application listens on port 8080.

## Requirements:
- Name: hr-web-app-service
- Type: NodePort
- Endpoints: 2
- NodePort: 30082
- Port: 8080


## Solution Steps

1. **Create the yaml file**

    Create a file named `hr-web-app-service.yaml` with the following content:
  
```yaml
apiVersion: v1
kind: Service
metadata:
 name: hr-web-app-service
spec:
  type: NodePort
  selector:
   app: hr-web-app
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30082
```

2. **Apply the service:**
   ```sh
   kubectl apply -f hr-web-app-service.yaml
   ```
  
3. **Verify the service:**
  ```sh
    kubectl get svc hr-web-app-service
    kubectl describe svc hr-web-app-service
    kubectl get endpoints hr-web-app-service
  ```

4. **Access the service:**
   - Get node IPs:
     ```sh
     kubectl get nodes -o wide
     ```

   - Check service from curl:
     ```sh
     curl http://<node-ip>:30082
     ```