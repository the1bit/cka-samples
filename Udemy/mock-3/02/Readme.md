# Mock Exam 2 - 02

## Task: Create Service Account and ClusterRole

Create a new service account with the name pvviewer. Grant this Service account access to list all PersistentVolumes in the cluster by creating an appropriate cluster role called pvviewer-role and ClusterRoleBinding called pvviewer-role-binding.
Next, create a pod called pvviewer with the image: redis and serviceAccount: pvviewer in the default namespace.

## Requirements

- ServiceAccount: pvviewer
- ClusterRole: pvviewer-role
- ClusterRoleBinding: pvviewer-role-binding
- Pod: pvviewer

Is the pod configured to use ServiceAccount pvviewer?

# Documentation

- [Kubernetes Service Accounts](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)
- [Kubernetes Roles and RoleBindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-rolebinding)

## Solution Steps

Pods authenticate to the API Server using ServiceAccounts. If the serviceAccount name is not specified, the default service account for the namespace is used during a pod creation.

Reference: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

Now, create a service account pvviewer:

```bash
kubectl create serviceaccount pvviewer
```

To create a clusterrole:

```bash
kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list
```

To create a clusterrolebinding:

```bash
kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```

Solution manifest file to create a new pod called pvviewer as follows:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pvviewer
  name: pvviewer
spec:
  containers:
    - image: redis
      name: pvviewer
  # Add service account name
  serviceAccountName: pvviewer
```
