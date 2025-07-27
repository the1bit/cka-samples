# Mock Exam 3 - 01

## Task: Kubecrnetes deployment with kubeadm

You are an administrator preparing your environment to deploy a Kubernetes cluster using kubeadm. Adjust the following network parameters on the system to the following values, and make sure your changes persist reboots:
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-iptables = 1

## Requirements:

- net.ipv4.ip_forward is set to 1
- net.bridge.bridge-nf-call-iptables is set to 1

## Documentation:

- [Kubernetes kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [Kubernetes networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

## Solution steps:

1. Edit the sysctl configuration file to set the required parameters:

```bash
sudo vi /etc/sysctl.conf
```

2. Add or modify the following lines in the file:

```bash
net.ipv4.ip_forward=1
net.bridge.bridge-nf-call-iptables=1
```

3. Save the file and exit the editor.

```bash
sudo sysctl -p
```

4. Verify the changes by running:

```bash
sysctl net.ipv4.ip_forward
sysctl net.bridge.bridge-nf-call-iptables
```

5. To ensure the changes persist after reboot, you can also run:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
```

6. Finally, verify the settings again to confirm they are applied correctly:

```bash
sysctl net.ipv4.ip_forward
sysctl net.bridge.bridge-nf-call-iptables
```
