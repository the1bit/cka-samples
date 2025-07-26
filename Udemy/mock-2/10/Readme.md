# Mock Exam 2 - 10

## Task: Uninstall helm chart

On the cluster, the team has installed multiple helm charts on a different namespace. By mistake, those deployed resources include one of the vulnerable images called kodekloud/webapp-color:v1. Find out the release name and uninstall it.


## Requirements:
- Is helm release uninstalled?

## Documentation:
- [Helm Uninstall](https://helm.sh/docs/helm/helm_uninstall/) 

## Solution Steps

1. List all helm releases in the cluster:

```bash
helm list --all-namespaces
```
_Result:_
```bash
NAME                    NAMESPACE               REVISION        UPDATED                                       STATUS    CHART                           APP VERSION
atlanta-page-apd        atlanta-page-04         1               2025-07-26 21:01:00.211434448 +0000 UTC       deployed  atlanta-page-apd-0.1.0          1.16.0     
digi-locker-apd         digi-locker-02          1               2025-07-26 21:00:58.503179999 +0000 UTC       deployed  digi-locker-apd-0.1.0           1.16.0     
security-alpha-apd      security-alpha-01       1               2025-07-26 21:00:57.983659217 +0000 UTC       deployed  security-alpha-apd-0.1.0        1.16.0     
web-dashboard-apd       web-dashboard-03        1               2025-07-26 21:00:59.00531129 +0000 UTC        deployed  web-dashboard-apd-0.1.0         1.16.0     
``` 

2. Identify the release name that includes the vulnerable image `kodekloud/webapp-color:v1`. 

```bash
kubectl get pods -n atlanta-page-04 -o yaml | grep kodekloud/webapp-color:v1
kubectl get pods -n digi-locker-02 -o yaml | grep kodekloud/webapp-color:v1
kubectl get pods -n security-alpha-01 -o yaml | grep kodekloud/webapp-color:v1
kubectl get pods -n web-dashboard-03 -o yaml | grep kodekloud/webapp-color:v1
```

_Result:_
```bash
kubectl get pods -n atlanta-page-04 -o yaml | grep kodekloud/webapp-color:v1
    - image: kodekloud/webapp-color:v1
      image: docker.io/kodekloud/webapp-color:v1
    - image: kodekloud/webapp-color:v1
      image: docker.io/kodekloud/webapp-color:v1
    - image: kodekloud/webapp-color:v1
      image: docker.io/kodekloud/webapp-color:v1
    - image: kodekloud/webapp-color:v1
      image: docker.io/kodekloud/webapp-color:v1
```

3. Uninstall the helm release using the following command:

```bash
helm uninstall atlanta-page-apd -n atlanta-page-04
```

4. Verify that the helm release is uninstalled by running:

```bash
helm list --all-namespaces
```