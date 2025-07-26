# Mock Exam 2 - 07

## Task: Static pod

Create a static pod on node01 called nginx-critical with the image nginx. Make sure that it is recreated/restarted automatically in case of a failure.


For example, use /etc/kubernetes/manifests as the static Pod path.

## Requirements:
- Is the static pod configured under /etc/kubernetes/manifests?
- Is pod nginx-critical-cluster1-node01 up and running?

## Documentation:
- [Kubernetes Static Pods](https://kubernetes.io/docs/tasks/configure-pod-containers/configure-pod-in-node/)


## Solution Steps

1. ssh into node01:

```bash
ssh node01  
```

2. Create a file named `nginx-critical.yaml` in the `/etc/kubernetes/manifests` directory with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-critical
  namespace: default
spec:
  containers:
  - name: nginx
    image: nginx
  restartPolicy: Always
``` 

3. Save the file and exit the editor.
4. Verify that the static pod is created and running by executing:

```bash
kubectl get pods -n default
```


