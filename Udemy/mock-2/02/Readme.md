# Miock Exam 2 - 02

## Task: Create Deployment

Create a deployment named logging-deployment in the namespace logging-ns with replica 1, with the following specifications:
The main container should be named app-container , use the image busybox , and should run the following command simulate writing logs:

```bash
sh -c "while true; do echo 'Log entry' >> /var/log/app/app.log; sleep 5; done"
```

Add a sidecar container named log-agent thet also uses the busybox image and runs the command:

```bash
tail -f /var/log/app/app.log
```

log-agent logs should display the entries logged by the main container.

Ensure that the main container mounts a volume at /var/log/app and that the sidecar app-container.

## Requirements:
- Sidecar displays logs from main container
- Sidecar container properly configured


## Documentation:
- [Kubernetes Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Kubernetes Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

## Solution steps:

1. Create a file named `deployment.yaml` with the following content:

```yaml
# logger-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: logging-deployment
  namespace: logging-ns
spec:
  replicas: 1
  selector:
    matchLabels:
      app: logger
  template:
    metadata:
      labels:
        app: logger
    spec:
      volumes:
        - name: log-volume
          emptyDir: {}
      initContainers:
        - name: log-agent
          image: busybox
          command:
            - sh
            - -c
            - "touch /var/log/app/app.log; tail -f /var/log/app/app.log"
          volumeMounts:
            - name: log-volume
              mountPath: /var/log/app
          restartPolicy: Always 
      containers:
        - name: app-container
          image: busybox
          command:
            - sh
            - -c
            - "while true; do echo 'Log entry' >> /var/log/app/app.log; sleep 5; done"
          volumeMounts:
            - name: log-volume
              mountPath: /var/log/app

```


2. Apply the configuration using the command:

```bash
kubectl apply -f deployment.yaml
```

3. Verify the deployment and pods:

```bash
kubectl get deployments -n logging-ns
kubectl get pods -n logging-ns
```

4. Check the logs of the sidecar container to ensure it is displaying the log entries from the main container:

```bash
kubectl logs -n logging-ns <pod-name> -c log-agent
```