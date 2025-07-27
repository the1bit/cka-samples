# Mock Exam 3 - 07

## Task: Taint and Toleration

Taint the worker node node01 to be Unschedulable. Once done, create a pod called dev-redis, image redis:alpine, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called prod-redis and image: redis:alpine with toleration to be scheduled on node01.

key: env_type, value: production, operator: Equal and effect: NoSchedule

## Requirements:

- key = env_type
- value = production
- effect = NoSchedule
- Is pod 'dev-redis' (no tolerations) not scheduled on node01?
- Is the 'prod-redis' to running on node01?

## Documentation:

- [Kubernetes Taints and Tolerations](https://kubernetes.io/docs/concepts/scheduling/taint-and-toleration/)
- [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

## Solution Steps

To add taints on the node01 worker node:

```bash
kubectl taint node node01 env_type=production:NoSchedule
```

Now, deploy dev-redis pod and to ensure that workloads are not scheduled to this node01 worker node.

```bash
kubectl run dev-redis --image=redis:alpine
```

To view the node name of recently deployed pod:

```bash
kubectl get pods -o wide
```

Solution manifest file to deploy new pod called prod-redis with toleration to be scheduled on node01 worker node.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: prod-redis
spec:
  containers:
    - name: prod-redis
      image: redis:alpine
  tolerations:
    - effect: NoSchedule
      key: env_type
      operator: Equal
      value: production
```

To view only prod-redis pod with less details:

```bash
kubectl get pods -o wide | grep prod-redis
```
