# Install Kubernetes Single Node on Ubuntu 22.04 LTS with kubeadm

## Prerequisites

- Ubuntu 22.04 LTS server
- Root or sudo privileges

## Steps

### 1. Configure version

```bash
KUBERNETES_VERSION=v1.33
CRIO_VERSION=v1.33
```

### 2. Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

### 3. Install the dependencies for adding repositories

```bash
sudo -i
apt-get update
apt-get install -y software-properties-common curl
```

### 4. Add Kubernetes Repository

```bash
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/$KUBERNETES_VERSION/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/$KUBERNETES_VERSION/deb/ /" |
    tee /etc/apt/sources.list.d/kubernetes.list

```

### 5. Add the CRI-O Repository

```bash
curl -fsSL https://download.opensuse.org/repositories/isv:/cri-o:/stable:/$CRIO_VERSION/deb/Release.key |
    gpg --dearmor -o /etc/apt/keyrings/cri-o-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/cri-o-apt-keyring.gpg] https://download.opensuse.org/repositories/isv:/cri-o:/stable:/$CRIO_VERSION/deb/ /" |
    tee /etc/apt/sources.list.d/cri-o.list

```

### 6. Install packages

```bash
apt-get update
apt-get install -y cri-o kubelet kubeadm kubectl
```

### 7. Enable and start CRI-O

```bash
systemctl start crio.service
systemctl enable crio.service
``` 

### 8. Bootstrap the Kubernetes cluster

```bash
swapoff -a
modprobe br_netfilter
sysctl -w net.ipv4.ip_forward=1

kubeadm init
```


### 9. Configure kubectl for the root user

```bash
export KUBECONFIG=/etc/kubernetes/admin.conf

kubectl get node
```

### 10. Install a CNI plugin

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

### 11. Verify the installation

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

### 12. Remove the taint from the master node

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

### 13. Run a test pod

```bash
kubectl run busybox --image=busybox --restart=Never -- sleep 3600
kubectl get pods
