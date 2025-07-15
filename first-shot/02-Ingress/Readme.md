# Ingress Solution

This document describes the Ingress configuration for the Kubernetes cluster.

## Overview

Ingress manages external access to services in a cluster, typically HTTP. It provides routing rules to manage traffic.

## Prerequisites

- Kubernetes cluster
- Ingress controller installed (e.g., NGINX Ingress Controller)


## Commands

```bash
# Create namespace for CKA Ingress
kubectl create namespace cka-ingress

# Create namespace form Ingress controller
kubectl create namespace ingress-nginx
# Apply the Ingress controller (e.g., NGINX)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

# Apply the Apache deployment
kubectl apply -f apache-deployment.yaml
# Apply the Ingress resource
kubectl apply -f apache-ingress.yaml
```

Related documentation:
- [Ingress Concepts](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [IngressClass - Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/#ingress-class)
- [Ingress v1 API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#ingress-v1-networking-k8s-io)