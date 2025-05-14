# kubeadm Cluster Creation 
1. Launch 3 instances
   - instance type: t3a.medium
   - AMI: ubuntu
   - Storage: 30GB
2. switch to root user sudo su -
3. Enable IPv4 packet forwarding
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
4. Install containerd Runtime
```bash
sudo apt install -y wget curl apt-transport-https ca-certificates gpg
wget https://github.com/containerd/containerd/releases/download/v2.0.0/containerd-2.0.0-linux-amd64.tar.gz -O /tmp/containerd.tar.gz
sudo tar -C /usr/local -xzf /tmp/containerd.tar.gz

wget https://github.com/opencontainers/runc/releases/download/v1.2.1/runc.amd64 -O /tmp/runc
sudo install -m 755 /tmp/runc /usr/local/sbin/runc

wget https://github.com/containernetworking/plugins/releases/download/v1.6.0/cni-plugins-linux-amd64-v1.6.0.tgz -O /tmp/cni.tgz
sudo mkdir -p /opt/cni/bin
sudo tar -xzf /tmp/cni.tgz -C /opt/cni/bin

mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -O /etc/systemd/system/containerd.service
sudo systemctl daemon-reexec
sudo systemctl enable --now containerd
```
5. Install Kubernetes Components
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | gpg --dearmor | sudo tee /etc/apt/keyrings/kubernetes-apt-keyring.gpg > /dev/null

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

sudo systemctl enable --now kubelet
```

6. Initialize Kubernetes Master[only on master node]
```bash
kubeadm init --pod-network-cidr=192.168.0.0/16
# after initializing you will get below 7 details
```
7. Create config to access cluster collects the after
```bash
# for non-root user
mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
# for root user
export KUBECONFIG=/etc/kubernetes/admin.conf
# to connect nodes as worker nodes we need join command
# run all worker nodes
kubeadm join 172.31.89.1:6443 --token gdh8mu.1ekcgo15ck21a3tm \
        --discovery-token-ca-cert-hash sha256:b39e74896fcb404eddb9cdff5263a53341655211557ab14f5ad768cbf0f9664a
```
8. To connect all nodes to single we need to install calico plugin
```bash
# Install Calico Plugin
# https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises
curl https://raw.githubusercontent.com/projectcalico/calico/v3.30.0/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```
# Commnads
- kubectl api-resources
- kubectl create ns siva
- kubectl delete ns siva
- kubectl describe ns siva

