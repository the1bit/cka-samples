# Mock Exam 1 - 05

## Task: Create a deployment

Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.

Requirements:
- Name: hr-web-app
- Image: kodekloud/webapp-color
- Replicas: 2

## Solution Steps

1a. **Create the deployment using kubectl:**

   ```sh
   kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas=2
   ```

1b. **Create the deployment yaml**

   Create a file named `hr-web-app-deployment.yaml` with the following content:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: hr-web-app
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: hr-web-app
     template:
       metadata:
         labels:
           app: hr-web-app
       spec:
         containers:
         - name: webapp-color
           image: kodekloud/webapp-color
   ```

2. **Apply the deployment:**
   ```sh
   kubectl apply -f hr-web-app-deployment.yaml
    ```

3. **Verify the deployment:**
    ```sh
    kubectl get deployments
    ```

