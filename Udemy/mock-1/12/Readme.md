# Mock Exam 1 - 12

## Task: Help deplyment

One co-worker deployed an nginx helm chart kk-mock1 in the kk-ns namespace, on the cluster. A new update is published to the helm chart, and the team wants you to update the helm repository to fetch the new changes.

After updateing the helm chart, upgrade the helm chart version to 18.1.15.

## Requirements:

- Is the deployment running?
- Is the chart version upgraded?

## Documentation:
- [Helm](https://helm.sh/docs/)

## Solution Steps

1. **List chart in the namespace:**

   ```sh
   helm list -n kk-ns
   ```

2. **List Helm repositories:**

   ```sh
   helm repo list
   ```

3. **Update Helm repositories:**

   ```sh
    helm repo update
   ```

4. **Search nginx chart:**

   ```sh
   helm search repo nginx
   ```

5. ** Serch for the chart version:**

   ```sh
   helm search repo nginx --versions
   ```

6. **Search for the 18.1.15 version:**

   ```sh
   helm search repo nginx --versions | grep 18.1.15

   # or

    helm search repo nginx --version 18.1.15
   ```

7. **Upgrade the helm chart:**

   ```sh
    helm upgrade kk-mock1 kk-mock1/nginx --version 18.1.15 -n kk-ns
   ```

8. **Verify the deployment:**
   ```sh
   helm list -n kk-ns
   ```
