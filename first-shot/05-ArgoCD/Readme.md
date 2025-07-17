# Install ArgoCD with Helm

Follow these steps to install ArgoCD using Helm:

## Prerequisites

- [Helm](https://helm.sh/docs/intro/install/) installed
- Access to a Kubernetes cluster

## Create Namespace for ArgoCD

```bash
kubectl create namespace cka-argocd
```

## Check Installed ArgoCD CRDs

After installation, verify that ArgoCD Custom Resource Definitions (CRDs) are present:

```bash
kubectl get crds | grep argoproj.io
```

## Add ArgoCD Helm Repository

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

## Install ArgoCD

```bash
helm install argocd argo/argo-cd \
  --namespace cka-argocd \
  --version 5.46.8 \
  --set server.service.type=ClusterIP
```

_Note: If CRDs already exist, you can skip the CRD installation step by using `--skip-crds` flag._

## Verify Installation

```bash
kubectl get pods -n cka-argocd
```

## Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n cka-argocd 8080:443
```

Then open [https://localhost:8080](https://localhost:8080) in your browser.

## Get Initial Admin Password

```bash
kubectl get secret argocd-initial-admin-secret -n cka-argocd -o jsonpath="{.data.password}" | base64 -d
```

> **Note:** For production, review [ArgoCD Helm chart documentation](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd) for best practices and configuration options.

## Upgrade ArgoCD

To upgrade ArgoCD to a newer version, you can use the following command:

```bash
helm upgrade argocd argo/argo-cd \
  --namespace cka-argocd \
  --version 8.1.3 \
  --set server.service.type=ClusterIP

```

## Documentation

- [ArgoCD Helm Install Guide](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd)
- [ArgoCD CRDs Info](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/)
- [Helm Chart values](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd)
- [ArgoCD install options](https://argo-cd.readthedocs.io/en/stable/getting_started/)
