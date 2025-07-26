# Mock Exam 2 - 05


## Task: RBAC

Create a new user called john. Grant him access to the cluster using a csr named john-developer. Create a role developer which should grant John the permission to create, list, get, update and delete pods in the development namespace . The private key exists in the location: /root/CKA/john.key and csr at /root/CKA/john.csr.


Important Note: As of kubernetes 1.19, the CertificateSigningRequest object expects a signerName.

Please refer to the documentation to see an example. The documentation tab is available at the top right of the terminal.

## Requirements:
- CSR: john-developer Status:Approved
- Role Name: developer, namespace: development, Resource: Pods
- Access: User 'john' has appropriate permissions


## Documentation:
- [Kubernetes RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- [Kubernetes CertificateSigningRequest](https://kubernetes.io/docs/reference/generated/kube-apiserver/#certificatesigningrequest)

## Solution steps:

1. Manually Base64 Encode the CSR File: Run the following command to base64-encode the john.csr file and remove newline characters:

```bash
cat /root/CKA/john.csr | base64 | tr -d '\n'
```

2. Update the csr.yaml File: Replace the placeholder $(cat /root/CKA/john.csr | base64 | tr -d '\n') in the request field with the actual base64-encoded content. The updated csr.yaml should look like this:

```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: <result from 1>
  usages:
  - digital signature
  - key encipherment
  - client auth
```
2. Apply the CSR configuration using the command:

```bash
kubectl apply -f csr.yaml
```

3. Approve the CSR:

```bash
kubectl certificate approve john-developer
```

4. Next, create a role developer and rolebinding developer-role-binding, run the command:

```bash
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development

kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```

5. To verify the permission from kubectl utility tool:

```bash
kubectl auth can-i update pods --as=john --namespace=development
```