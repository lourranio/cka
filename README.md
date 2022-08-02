# cka
Certified Kubernetes Administrator (CKA)

https://landscape.cncf.io/
https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/

## Cluster Architecture, Installation & Configuration25%
# Instalação com o KUBEADM + WAVE NET
1. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

PREREQUISITOS
https://kubernetes.io/docs/setup/production-environment/container-runtimes/
```
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
```

1. Install ubuntu ( server ) + 2 ubuntus (workers)

https://docs.docker.com/engine/install/ubuntu/

 sudo apt-get remove docker docker-engine docker.io containerd runc

 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings

 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    sudo apt-get update

   sudo apt-get install containerd.io

   systemctl status containerd

2. ContainerD
https://github.com/containerd/containerd/blob/main/docs/getting-started.md

2.1 installing-cni-plugins
https://github.com/containerd/containerd/blob/main/docs/getting-started.md#step-3-installing-cni-plugins

    mkdir -p /opt/cni/bin

    wget https://github.com/containernetworking/plugins/releases/download/v1.1.1/cni-plugins-linux-amd64-v1.1.1.tgz



3. 
vim /etc/containerd/config.toml

    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true

systemctl enable containerd
systemctl restart containerd
systemctl statusapt containerd

4. 

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl


kubeadm config images pull --kubernetes-version=1.24.3

init 6

kubeadm init

Com usuario comum,
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

kubectl get nodes

5. Instalação do plug in de rede
https://kubernetes.io/docs/concepts/cluster-administration/addons/
https://www.weave.works/docs/net/latest/kubernetes/kube-addon/

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

PROXIMO : ADD NOVOS WORKERS ( nÓS)
