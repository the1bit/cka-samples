# CKA Simulator B Kubernetes 1.33


Question 1:

Solve this question on: ssh cka6016

The Deployment controller in Namespace lima-control communicates with various cluster internal endpoints by using their DNS FQDN values.

Update the ConfigMap used by the Deployment with the correct FQDN values for:

DNS_1: Service kubernetes in Namespace default
DNS_2: Headless Service department in Namespace lima-workload
DNS_3: Pod section100 in Namespace lima-workload. It should work even if the Pod IP changes
DNS_4: A Pod with IP 1.2.3.4 in Namespace kube-system
Ensure the Deployment works with the updated values.

ℹ️ You can use nslookup inside a Pod of the controller Deployment


## Solution

### FQDN Values

- **DNS_1:** `kubernetes.default.svc.cluster.local`
- **DNS_2:** `department.lima-workload.svc.cluster.local`
- **DNS_3:** `section100.lima-workload.pod.cluster.local`
- **DNS_4:** `1-2-3-4.kube-system.pod.cluster.local`

### Steps

1. **Edit the ConfigMap** used by the Deployment:
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: <configmap-name>
    namespace: lima-control
  data:
    DNS_1: kubernetes.default.svc.cluster.local
    DNS_2: department.lima-workload.svc.cluster.local
    DNS_3: section100.lima-workload.pod.cluster.local
    DNS_4: 1-2-3-4.kube-system.pod.cluster.local
  ```
2. **Apply the updated ConfigMap:**
  ```sh
  kubectl apply -f <configmap-file>.yaml
  ```
3. **Restart the Deployment** (if necessary) to reload the ConfigMap:
  ```sh
  kubectl rollout restart deployment <deployment-name> -n lima-control
  ```
4. **Verify** inside a Pod:
  ```sh
  kubectl exec -n lima-control <pod-name> -- nslookup $DNS_1
  ```

Replace `<configmap-name>`, `<configmap-file>.yaml`, `<deployment-name>`, and `<pod-name>` with actual resource names.



------
Question 2:

Solve this question on: ssh cka2560

Create a Static Pod named my-static-pod in Namespace default on the controlplane node. It should be of image nginx:1-alpine and have resource requests for 10m CPU and 20Mi memory.

Create a NodePort Service named static-pod-service which exposes that static Pod on port 80.

ℹ️ For verification check if the new Service has one Endpoint. It should also be possible to access the Pod via the cka2560 internal IP address, like using curl 192.168.100.31:NODE_PORT

## Solution

### 1. Create the Static Pod manifest

Save the following YAML as `/etc/kubernetes/manifests/my-static-pod.yaml` on the controlplane node:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-static-pod
  namespace: default
spec:
  containers:
    - name: nginx
      image: nginx:1-alpine
      resources:
        requests:
          cpu: "10m"
          memory: "20Mi"
```

### 2. Verify the Static Pod is running

```sh
kubectl get pod my-static-pod -n default
```

### 3. Create a NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: static-pod-service
  namespace: default
spec:
  type: NodePort
  selector:
    # Static Pods are not managed by a controller, so use manual selector
    # The Pod must have the label below; add it if missing
    app: my-static-pod
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080 # Or omit for auto-assignment
```

Add the label to the Pod (edit the manifest):

```yaml
metadata:
  labels:
    app: my-static-pod
```

Apply the Service:

```sh
kubectl apply -f <service-file>.yaml
```

### 4. Verify Service and Endpoint

```sh
kubectl get endpoints static-pod-service -n default
kubectl get svc static-pod-service -n default
```

### 5. Test access

```sh
curl 192.168.100.31:<NODE_PORT>
```

Replace `<NODE_PORT>` with the actual NodePort assigned.


---
Question 3:
Solve this question on: ssh cka5248

Node cka5248-node1 has been added to the cluster using kubeadm and TLS bootstrapping.

Find the Issuer and Extended Key Usage values on cka5248-node1 for:

Kubelet Client Certificate, the one used for outgoing connections to the kube-apiserver
Kubelet Server Certificate, the one used for incoming connections from the kube-apiserver
Write the information into file /opt/course/3/certificate-info.txt.

ℹ️ You can connect to the worker node using ssh cka5248-node1 from cka5248

## Solution

### 1. Locate the Kubelet Certificates

On `cka5248-node1`, the kubelet certificates are typically found in `/var/lib/kubelet/pki/`:

- **Client certificate:** `/var/lib/kubelet/pki/kubelet-client-current.pem`
- **Server certificate:** `/var/lib/kubelet/pki/kubelet.crt`

### 2. Extract Issuer and Extended Key Usage

Run the following commands on `cka5248-node1`:

```sh
# Kubelet Client Certificate
echo "Kubelet Client Certificate:" > /opt/course/3/certificate-info.txt
openssl x509 -in /var/lib/kubelet/pki/kubelet-client-current.pem -noout -issuer -ext extendedKeyUsage >> /opt/course/3/certificate-info.txt

# Kubelet Server Certificate
echo -e "\nKubelet Server Certificate:" >> /opt/course/3/certificate-info.txt
openssl x509 -in /var/lib/kubelet/pki/kubelet.crt -noout -issuer -ext extendedKeyUsage >> /opt/course/3/certificate-info.txt
```

### 3. Verify the Output

Check the contents of `/opt/course/3/certificate-info.txt` to ensure both Issuer and Extended Key Usage are recorded for each certificate.


---
Question 4:

Solve this question on: ssh cka3200

Do the following in Namespace default:

Create a Pod named ready-if-service-ready of image nginx:1-alpine
Configure a LivenessProbe which simply executes command true
Configure a ReadinessProbe which does check if the url http://service-am-i-ready:80 is reachable, you can use wget -T2 -O- http://service-am-i-ready:80 for this
Start the Pod and confirm it isn't ready because of the ReadinessProbe.
Then:

Create a second Pod named am-i-ready of image nginx:1-alpine with label id: cross-server-ready
The already existing Service service-am-i-ready should now have that second Pod as endpoint
Now the first Pod should be in ready state, check that



---
Question 5:

Solve this question on: ssh cka8448

Create two bash script files which use kubectl sorting to:

Write a command into /opt/course/5/find_pods.sh which lists all Pods in all Namespaces sorted by their AGE (metadata.creationTimestamp)

Write a command into /opt/course/5/find_pods_uid.sh which lists all Pods in all Namespaces sorted by field metadata.uid


---
Question 6:

Solve this question on: ssh cka1024

There seems to be an issue with the kubelet on controlplane node cka1024, it's not running.

Fix the kubelet and confirm that the node is available in Ready state.

Create a Pod called success in default Namespace of image nginx:1-alpine.

ℹ️ The node has no taints and can schedule Pods without additional tolerations


---
Question 7:
Solve this question on: ssh cka2560

You have been tasked to perform the following etcd operations:

Run etcd --version and store the output at /opt/course/7/etcd-version
Make a snapshot of etcd and save it at /opt/course/7/etcd-snapshot.db

---
Question 8:

Solve this question on: ssh cka8448

Check how the controlplane components kubelet, kube-apiserver, kube-scheduler, kube-controller-manager and etcd are started/installed on the controlplane node.

Also find out the name of the DNS application and how it's started/installed in the cluster.

Write your findings into file /opt/course/8/controlplane-components.txt. The file should be structured like:

# /opt/course/8/controlplane-components.txt
kubelet: [TYPE]
kube-apiserver: [TYPE]
kube-scheduler: [TYPE]
kube-controller-manager: [TYPE]
etcd: [TYPE]
dns: [TYPE] [NAME]
Choices of [TYPE] are: not-installed, process, static-pod, pod

---
Question 9:
Solve this question on: ssh cka5248

Temporarily stop the kube-scheduler, this means in a way that you can start it again afterwards.

Create a single Pod named manual-schedule of image httpd:2-alpine, confirm it's created but not scheduled on any node.

Now you're the scheduler and have all its power, manually schedule that Pod on node cka5248. Make sure it's running.

Start the kube-scheduler again and confirm it's running correctly by creating a second Pod named manual-schedule2 of image httpd:2-alpine and check if it's running on cka5248-node1.

---
Question 10:

Solve this question on: ssh cka6016

There is a backup Job which needs to be adjusted to use a PVC to store backups.

Create a StorageClass named local-backup which uses provisioner: rancher.io/local-path and volumeBindingMode: WaitForFirstConsumer. To prevent possible data loss the StorageClass should keep a PV retained even if a bound PVC is deleted.

Adjust the Job at /opt/course/10/backup.yaml to use a PVC which request 50Mi storage and uses the new StorageClass.

Deploy your changes, verify the Job completed once and the PVC was bound to a newly created PV.

ℹ️ To re-run a Job, delete it and create it again

ℹ️ The abbreviation PV stands for PersistentVolume and PVC for PersistentVolumeClaim

---
Question 11:

Solve this question on: ssh cka2560

Create Namespace secret and implement the following in it:

Create Pod secret-pod with image busybox:1. It should be kept running by executing sleep 1d or something similar

Create the existing Secret /opt/course/11/secret1.yaml and mount it readonly into the Pod at /tmp/secret1

Create a new Secret called secret2 which should contain user=user1 and pass=1234. These entries should be available inside the Pod's container as environment variables APP_USER and APP_PASS


---
Question 12:

Solve this question on: ssh cka5248

Create a Pod of image httpd:2-alpine in Namespace default.

The Pod should be named pod1 and the container should be named pod1-container.

This Pod should only be scheduled on controlplane nodes.

Do not add new labels to any nodes.

---
Question 13:

Solve this question on: ssh cka3200

Create a Pod with multiple containers named multi-container-playground in Namespace default:

It should have a volume attached and mounted into each container. The volume shouldn't be persisted or shared with other Pods

Container c1 with image nginx:1-alpine should have the name of the node where its Pod is running on available as environment variable MY_NODE_NAME

Container c2 with image busybox:1 should write the output of the date command every second in the shared volume into file date.log. You can use while true; do date >> /your/vol/path/date.log; sleep 1; done for this.

Container c3 with image busybox:1 should constantly write the content of file date.log from the shared volume to stdout. You can use tail -f /your/vol/path/date.log for this.

ℹ️ Check the logs of container c3 to confirm correct setup

---
Question 14:


Solve this question on: ssh cka8448

You're ask to find out following information about the cluster:

How many controlplane nodes are available?
How many worker nodes (non controlplane nodes) are available?
What is the Service CIDR?
Which Networking (or CNI Plugin) is configured and where is its config file?
Which suffix will static pods have that run on cka8448?
Write your answers into file /opt/course/14/cluster-info, structured like this:

# /opt/course/14/cluster-info
1: [ANSWER]
2: [ANSWER]
3: [ANSWER]
4: [ANSWER]
5: [ANSWER]


---
Question 15:

Solve this question on: ssh cka6016

Write a kubectl command into /opt/course/15/cluster_events.sh which shows the latest events in the whole cluster, ordered by time (metadata.creationTimestamp)
Delete the kube-proxy Pod and write the events this caused into /opt/course/15/pod_kill.log on cka6016
Manually kill the containerd container of the kube-proxy Pod and write the events into /opt/course/15/container_kill.log

---
Question 16:

Solve this question on: ssh cka3200

Write the names of all namespaced Kubernetes resources (like Pod, Secret, ConfigMap...) into /opt/course/16/resources.txt.

Find the project-* Namespace with the highest number of Roles defined in it and write its name and amount of Roles into /opt/course/16/crowded-namespace.txt.


---
Question 17:

Solve this question on: ssh cka6016

There is Kustomize config available at /opt/course/17/operator. It installs an operator which works with different CRDs. It has been deployed like this:

kubectl kustomize /opt/course/17/operator/prod | kubectl apply -f -
Perform the following changes in the Kustomize base config:

The operator needs to list certain CRDs. Check the logs to find out which ones and adjust the permissions for Role operator-role
Add a new Student resource called student4 with any name and description
Deploy your Kustomize config changes to prod.



