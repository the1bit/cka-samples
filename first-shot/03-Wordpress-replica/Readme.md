# Wordpress Replica

## Realistic Scenario

- You deployed 3 replicas of WordPress.
- Only 2 pods are Running, and 1 is Pending.
- Root cause: Cluster doesn't have enough free resources (CPU or memory) to schedule the 3rd pod.
- Your task is to:
  - Identify what the bottleneck is: CPU or memory?
  - Adjust resource requests (or limits) in your WordPress deployment to fit.
  - Ensure all 3 pods are running.

## Steps to investigate and resolve the issue

1. **Check the status of the pods**:

   ```bash
   kubectl get pods -n cka-wp
   ```

2. **Describe the pending pod** to find out why it is not scheduled:

   ```bash
    kubectl describe pod <pending-pod-name> -n cka-wp
   ```

3. **Check the resource requests and limits** of the WordPress deployment:

   ```bash
   kubectl get deployment wordpress -n cka-wp -o yaml
   ```

4. **Identify the bottleneck**:

   - Look for the `Resources` section in the pod description or deployment YAML.
   - Check the `Requests` and `Limits` for CPU and memory.
   - Check node resources to see if there are enough available resources:
   ```bash
    kubectl top nodes
    ```

5. **Adjust the resource requests or limits**:
   - If CPU is the bottleneck, reduce the CPU requests or limits in the deployment YAML
   - If memory is the bottleneck, reduce the memory requests or limits in the deployment YAML
   - Apply the changes:
   ```bash
   kubectl apply -f <modified-deployment-file>.yaml -n cka-wp
   ```
    _Optional: If you want to edit the deployment directly, you can use:_
    ```bash
    kubectl edit deployment wordpress -n cka-wp
    ```

6. **Verify the changes**:
   - Check the status of the pods again:
   ```bash
   kubectl get pods -n cka-wp
   ```


Related documentation:

- [Resource Requests & Limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
- [Troubleshoot Pod Scheduling](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduler-performance-tuning/)
- [Managing Resources for Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
- [Deployment Troubleshooting](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#troubleshooting)