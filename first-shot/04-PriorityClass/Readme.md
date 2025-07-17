# PriorityClass and Deployment Assignment


## Objective

Create a custom PriorityClass named high-priority with a value of 100000 and assign it to an existing deployment. This ensures the scheduler prioritizes this workload during resource contention.

## Practice Scenario
You need to:
- Create a PriorityClass named high-priority
- Set value: 100000
- Add a description (optional but good practice)
- Modify the deployment important-app (in namespace cka-priority) to use this PriorityClass


## Commands

```bash
kubectl create namespace cka-priority
```

```bash
# Create deployment
kubectl apply -f deployment.yaml
```

```bash
# Create PriorityClass
kubectl apply -f priority-class.yaml
```

Modify the deployment to use the created PriorityClass:

```yaml
...
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
      priorityClassName: high-priority
...
``` 

```bash
# Apply the modified deployment
kubectl apply -f deployment.yaml
```

```bash
# Check PriorityClass exists
kubectl get priorityclass

# View the deployment and confirm it's using the right priority
kubectl get pod -n cka-priority -o jsonpath="{.items[*].spec.priorityClassName}"

# Check the priority of the pods
kubectl get pod -n cka-priority -o jsonpath="{.items[*].spec.priority}"
```

## Documentation

- [Pod Priority and Preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)
- [PriorityClass API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#priorityclass-v1-scheduling-k8s-io)
