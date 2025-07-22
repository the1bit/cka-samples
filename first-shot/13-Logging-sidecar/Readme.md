# Logging Sidecar Example

## Official Documentation Reference

- [Kubernetes Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [Sidecar Containers](https://kubernetes.io/docs/concepts/workloads/pods/#using-pods)

## Scenario

You have a legacy application that writes logs to local files. To integrate with Kubernetes logging, you can use a **sidecar container** that tails the log file and streams it to stdout.

### Create namespace

```bash
kubectl apply -f 01-namespace.yaml
```

## Create legacy application

```bash
kubectl apply -f 02-legacy-app.yaml
```

### Check logs

```bash
kubectl -n cka-sidecar exec legacy-app -c legacy -- tail -5 /var/log/app.log
```

### Delete legacy application

```bash
kubectl delete pod legacy-app -n cka-sidecar
```

## Create legacy application with logging sidecar

```bash
kubectl apply -f 03-legacy-app-with-sidecar.yaml
``` 

### Check logs from sidecar

```bash
kubectl -n cka-sidecar logs legacy-app -c log-sidecar
```

## Key Points

- **Sidecar container** tails the log file and outputs to stdout.
- Use a **shared volume** (`emptyDir`) for log file access.
- Kubernetes collects logs from container stdout/stderr.

## Exam Preparation Tips

- Understand how to use sidecars for logging.
- Know how to share volumes between containers.
- Review [Kubernetes logging best practices](https://kubernetes.io/docs/concepts/cluster-administration/logging/#best-practices).
