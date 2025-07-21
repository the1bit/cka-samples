# Migrate an Ingress Resource to Gateway API


This document describes how to migrate an existing Ingress resource to the Gateway API in a Kubernetes cluster.

## Overview
The Gateway API is a set of resources that provide a more expressive and extensible way to manage ingress traffic in Kubernetes. It allows for better control over routing, policies, and traffic management compared to the traditional Ingress resource.

## Prerequisites
- A Kubernetes cluster with the Gateway API installed.
- An existing Ingress resource that you want to migrate.
- Familiarity with Kubernetes resources and YAML configuration.

## Modify local computer settings to access the Ingress

To access the Ingress from your local computer, you need to modify your `/etc/hosts` file (Linux/Mac) or `C:\Windows\System32\drivers\etc\hosts` file (Windows) to map the Ingress hostnames to the IP address of your 

## Steps to Migrate Ingress to Gateway API

### Install Gateway API CRD
Ensure that the Gateway API Custom Resource Definitions (CRDs) are installed in your cluster. You can do this by applying the following command:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.1.0/standard-install.yaml
```






## Documentation

- [Migrate Ingress to Gateway API (K8s Docs)](https://kubernetes.io/docs/concepts/services-networking/gateway/#migrating-from-ingress)
- [HTTPRoute Spec](https://gateway-api.sigs.k8s.io/references/spec/#gateway.networking.k8s.io/v1.HTTPRoute)
- [Gateway API Concepts](https://kubernetes.io/docs/concepts/services-networking/gateway/)