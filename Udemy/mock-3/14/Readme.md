# Mock Exam 3 - 14

## Task: Identify Pod CIDR Network

Identify the pod CIDR network of the node 'controlplane' in the kubernetes cluster. This information is crucial for configuring the CNI plugin during installation. Output the pod CIDR network to a file at /root/pod-cidr.txt.

## Requirements:

- Is Pod CIDR network correctly outputted to file?

## Documentation:

- [Kubernetes Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
- [Kubernetes Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)

## Solution Steps

Use kubectl cluster-info to find details about the cluster and check the network range for pods.

To identify the Pod CIDR network of the Kubernetes cluster, use the following command:

```bash
kubectl get node controlplane -o jsonpath="{.spec.podCIDR}" > /root/pod-cidr.txt
```

To verify:

```bash
cat /root/pod-cidr.txt
```
