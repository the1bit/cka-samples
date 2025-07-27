# Mock Exam 3 - 09

## Task: Fix Kubeconfig

A kubeconfig file called super.kubeconfig has been created under /root/CKA. There is something wrong with the configuration. Troubleshoot and fix it.

## Requirements:

- Fix /root/CKA/super.kubeconfig

## Documentation:

- [Kubernetes Kubeconfig](https://kubernetes.io/docs/concepts/configuration/kubeconfig/)

## Solution Steps

Verify host and port for kube-apiserver are correct.

Open the super.kubeconfig in vi editor.

```bash
vi /root/CKA/super.kubeconfig
```

Change the 9999 port to 6443 and run the below command to verify:

```bash
kubectl cluster-info --kubeconfig=/root/CKA/super.kubeconfig
```
