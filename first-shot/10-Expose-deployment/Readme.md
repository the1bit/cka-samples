# Exposing a Deployment with NodePort Service (Command Line Approach)

## Official Documentation References

- [Expose your app publicly using a NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#nodeport)
- [kubectl create deployment](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create)
- [Using kubectl to create a Deployment](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)
- [kubectl expose](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose)

## Step-by-Step Solution

### 1. Create a Deployment from Command Line

```sh
kubectl create deployment nginx --image=nginx
```

### 2. Expose Deployment with NodePort

```sh
kubectl -n cka-expose expose deployment nginx --port=80 --target-port=80 --type=NodePort
```

This exposes port 80 of the deployment using a NodePort service.

### 3. Verify the Service

```sh
kubectl get svc
```

Check the `NODEPORT` assigned and access the service via `<NodeIP>:<NodePort>`.

## Key Points for CKA

- Use `kubectl create deployment` and `kubectl expose` for fast resource creation.
- NodePort exposes the service on each Node’s IP at a static port.
- Always refer to official docs for command syntax.

## Practice Task

1. Create a deployment with a web server (nginx) using the command line.
2. Expose it with a NodePort service on port 80.
3. Access the service using the node’s IP and assigned NodePort.

