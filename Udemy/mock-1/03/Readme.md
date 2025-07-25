# # Mock Exam 1 - 03


## Task: Export CRD to file

On controlplane node, identify al CRD related to VericalPodAutoscaler and save their names into the file /root/vpa-crd.txt. 

Requirements:
- Does the file contain correct CRDs?


## Solution Steps

1. **List all CRDs**
   ```sh
   kubectl get crd
   ```

2. **Filter for VerticalPodAutoscaler CRDs**
   ```sh
   kubectl get crd | grep verticalpodautoscaler
   ```

3. **Save the names to a file**
   ```sh
   kubectl get crd | grep verticalpodautoscaler | awk '{print $1}' > /root/vpa-crd.txt
   ```