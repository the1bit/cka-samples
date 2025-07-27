# Mock Exam 3 - 13

## Task: Upgrade Helm Release

One application, webpage-server-01, is currently deployed on the Kubernetes cluster using Helm. A new version of the application is available in a Helm chart located at /root/new-version.

Validate this new Helm chart, then install it as a new release named webpage-server-02. After confirming the new release is installed, uninstall the old release webpage-server-01.

## Requirements:

- Is the new version app deployed?
- Is the old version app uninstalled?

## Documentation:

- [Helm Install](https://helm.sh/docs/helm/helm_install/)
- [Helm Uninstall](https://helm.sh/docs/helm/helm_uninstall/
- [Helm Lint](https://helm.sh/docs/helm/helm_lint/)
- [Helm List](https://helm.sh/docs/helm/helm_list/)
- [Helm Upgrade](https://helm.sh/docs/helm/helm_upgrade/)
- [Helm Rollback](https://helm.sh/docs/helm/helm_rollback/)

## Solution Steps

In this task, we will use the helm commands. Here are the steps:

Use the helm ls command to list the Helm releases in the default namespace.

```bash
helm ls -n default
```

Validate the Helm chart using the helm lint command:

```bash
cd /root/
helm lint ./new-version
```

Install the new version of the application as a new release named webpage-server-02:

```bash
helm install webpage-server-02 ./new-version
```

Uninstall the old release webpage-server-01 using the following command:

```bash
helm uninstall webpage-server-01 -n default
```
