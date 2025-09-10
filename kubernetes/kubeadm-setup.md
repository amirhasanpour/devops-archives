# **Kubeadm Setup**

For full documentation visit [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

---

## **1 - Change hostname and config swap for each node**

```bash
vi /etc/hostname
vi /etc/hostname
```

Then reboot the node for see the changes and after that disable swap:

```bash
swapoff -a
```

you can ensure that swap is off by using `free` command

---

## **2 - Enable IPv4 packet forwarding**

```bash
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```

Verify that net.ipv4.ip_forward is set to 1 with:

```bash
sysctl net.ipv4.ip_forward
```

---

## **3 - Install containerd for each node**

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

```bash
sudo apt-get install containerd.io
```

---

## **4 - Configuring the systemd cgroup driver**

```bash
containerd config default > /etc/containerd/config.toml
```

then open config.toml file

```bash
vi /etc/containerd/config.toml
```

in this file, set `SystemdCgroup = true`

at the final, restart containerd and check it status

```bash
systemctl restart containerd
systemctl status containerd
```

---

## 5 - Installing kubeadm, kubelet and kubectl

Update the `apt` package index and install packages needed to use the Kubernetes `apt` repository

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

Download the public signing key for the Kubernetes package repositories

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the appropriate Kubernetes `apt` repository

```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update the `apt` package index, install kubelet, kubeadm and kubectl, and pin their version

```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

---

## 5 - Initial the master node

in this case we are using `flannel`. you can use other things like `calico`

```bash
kubeadm init --apiserver-advertise-address <master_node_ip>  --pod-network-cidr 10.244.0.0/16 --cri-socket unix:///var/run/containerd/containerd.sock
```

after that, do this steps:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

## 6 - join other nodes to the master node

use kubeadm join token that recived in the privious step and join other nodes to the master node

use these two command to see that every thing is alright

```bash
kubectl get node
kubectl get pod -A
```

---

## 7 - Active core dns services on master node

```bash
kubectl apply -f https://reweave.azurewebsites.net/k8s/v1.32/net.yaml
```


