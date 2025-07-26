# Mock Exam 2 - 04

## Task: Create Deployment

Create a deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Next, upgrade the deployment to version 1.17 using rolling update.

## Requirements:
- Deployment created with nginx:1.16 image.
- Image: nginx:1.16
- Version upgraded to 1.17.


## Documentation:
- [Kubernetes Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Kubernetes Rolling Updates](https://kubernetes.io/docs/tutorials/kubernetes-basics/update-intro/)

## Solution steps:

1. Create a file named `deployment-upgrade.yaml` with the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80
```


2. Apply the configuration using the command:

```bash
kubectl apply -f deployment-upgrade.yaml
```

3. Verify the deployment is created with the correct image by running:

```bash
kubectl rollout history deployment/nginx-deploy --revision=1
```

4. Upgrade the deployment to version 1.17 using the command:

```bash
kubectl set image deployment/nginx-deploy nginx=nginx:1.17
```

5. Verify the upgrade by checking the rollout status:

```bash
kubectl rollout history deployment/nginx-deploy
```

6. Check the current image version to confirm the upgrade:

```bash
kubectl get deployment nginx-deploy -o=jsonpath='{.spec.template.spec.containers[0].image}'
```
