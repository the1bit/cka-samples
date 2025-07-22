# Export cert-manager CRDs to File

## Official Documentation Reference

- [Kubernetes: Custom Resource Definitions](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/)
- [kubectl: Exporting Resources](https://kubernetes.io/docs/reference/kubectl/overview/)
- [cert-manager: Installation](https://cert-manager.io/docs/installation/kubernetes/)

## Step-by-Step Solution

### 1. Install cert-manager

You can install cert-manager using kubectl and the official manifests:

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
```

Wait until all cert-manager pods are running:

```sh
kubectl get pods --namespace cert-manager
```

### 2. List cert-manager CRDs

```sh
kubectl get crd | grep cert-manager
```

### 3. Export a CRD to YAML

Replace `<crd-name>` with the actual CRD name from the previous step.

```sh
kubectl get crd <crd-name> -o yaml > <crd-name>.yaml
```

### 4. Export All cert-manager CRDs

You can use a loop to export all cert-manager CRDs:

```sh
for crd in $(kubectl get crd -o name | grep cert-manager); do
    kubectl get $crd -o yaml > $(echo $crd | cut -d'/' -f2).yaml
done
```

### 5. Export CRD Using JSONPath

You can use JSONPath to extract specific fields from CRDs. For example, to get the first cert-manager CRD object:

```sh
kubectl -n cert-manager get crd -o=jsonpath='{.items[*].metadata.name}'
```

Or, to list the names of all cert-manager CRDs:

```sh
kubectl get crd -o=jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}' | grep cert-manager
```



## Exam Tips

- Always use `kubectl get crd` to list CRDs.
- Use `-o yaml` for exporting in YAML format.
- Redirect output to a file using `>`.
- Practice with actual cert-manager CRDs in a test cluster.
- Know how to install cert-manager using official manifests.

## Example

```sh
kubectl get crd certificates.cert-manager.io -o yaml > certificates.cert-manager.io.yaml
```

## Summary

- Install cert-manager: `kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml`
- List CRDs: `kubectl get crd`
- Export CRD: `kubectl get crd <name> -o yaml > <name>.yaml`

This approach is simple, direct, and aligns with CKA exam requirements.

