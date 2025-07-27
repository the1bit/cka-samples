# Mock Exam 3 - 05

## Task: Create a PriorityClass

Create a PriorityClass named low-priority with a value of 50000. A pod named lp-pod exists in the namespace low-priority. Modify the pod to use the priority class you created. Recreate the pod if necessary.

## Requirements:

- Is the PriorityClass low-priority created?
- Low priority class value is set properly to 50000
- Pod lp-pod uses the low-priority PriorityClass

## Documentation:

- [Kubernetes Priority Classes](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)
- [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/

## Solution Steps

Use kubectl apply to create the PriorityClass, then edit the existing pod to add the priority class.

Create the priority class manifest file pc.yaml

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: low-priority
value: 50000
globalDefault: false
description: "Low priority class"
```

Apply the manifest file

```bash
kubectl apply -f pc.yaml
```

Inspect the pod definition to be able to recreate it:

```bash
kubectl get pod lp-pod -n low-priority -o yaml
```

Create and update the new yaml lp-pod.yaml for the pod after adding the priority class

```bash
vi lp-pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: lp-pod
  namespace: low-priority
spec:
  priorityClassName: low-priority
  containers:
    - name: nginx
      image: nginx
```

Apply the updated pod definition:

```bash
kubectl apply -f lp-pod.yaml
```

Delete and recreate the pod using the configured priority class

```bash
kubectl delete pod lp-pod -n low-priority
kubectl apply -f lp-pod.yaml
```
