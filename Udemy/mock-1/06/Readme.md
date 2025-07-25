# Mock Exam 1 - 06

## Task: Fix issue in deployment

A new application orange is deployed. There is something wrong with it. Identify the issue and fix it.

Requirements:

- Is the issue fixed?

## Solution Steps

1. **Check the deployment status:**
   ```sh
   kubectl describe pod orange
   ```

Result:

```
State: Terminated
  Reason: Error
  Exit Code: 127
```

2. **Check the logs of the pod:**
   ```sh
   kubectl logs orange -c init-myservice
   ```

Result:

```
sh: sleeeep: not found
```

3. **Edit the deployment to fix the command:**

   ```sh
   kubectl get pod orange -o yaml > orange-pod.yaml
   ```

4. **Modify the command in the init container:**
   Open `orange-pod.yaml` and change the command from `sleeeep` to `sleep`.

5. **Apply the changes:**

   ```sh
   kubectl replace -f orange-pod.yaml --force
   ```

6. **Verify the deployment:**

   ```sh
   kubectl get pods
   ```

   Check that the pod is running and the init container has completed successfully.
