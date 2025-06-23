
# 🚀 Kubernetes Cluster Setup with NGINX Deployment

This repository provides resources and configurations to set up Kubernetes clusters (single-node and multi-node) and deploy NGINX web servers on them.

---

## 📑 Table of Contents
- [Single Node Kubernetes Cluster Setup](#single-node-kubernetes-cluster-setup)
- [Multi Node Kubernetes Cluster Setup](#multi-node-kubernetes-cluster-setup)
- [NGINX Deployment on Kubernetes](#nginx-deployment-on-kubernetes)
- [Files in This Repository](#files-in-this-repository)
- [Usage Instructions](#usage-instructions)
- [Contributing](#contributing)
- [License](#license)

---

## 🖥️ Single Node Kubernetes Cluster Setup

### Prerequisites
- Ubuntu machine (physical or virtual)
- Minimum 2GB RAM
- 2 vCPUs
- 20GB disk space

### Installation Steps
1️⃣ Update system packages:
```bash
sudo apt update && sudo apt upgrade -y
```

2️⃣ Install Docker:
```bash
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

3️⃣ Install Kubernetes components:
```bash
sudo apt install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

4️⃣ Initialize the cluster:
```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

5️⃣ Set up kubeconfig:
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

6️⃣ Install Flannel networking:
```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

---

## 🌐 Multi Node Kubernetes Cluster Setup

### Prerequisites
- 1 master node (as above)
- 1+ worker nodes (≥1GB RAM each)
- All nodes on the same network

### Setup
🔹 On the master node:
```bash
kubeadm token create --print-join-command
```

🔹 On each worker node:
- Install Docker and Kubernetes (same steps as master)
- Run the `kubeadm join` command provided by the master

🔹 Verify cluster on master:
```bash
kubectl get nodes
```

---

## 🌟 NGINX Deployment on Kubernetes

### Imperative Method
```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
```

### Declarative Method
```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

### Accessing NGINX
- **ClusterIP**: Internal cluster access  
- **NodePort**: `http://<node-ip>:<node-port>`  
- **LoadBalancer**: If using cloud provider  

Get service info:
```bash
kubectl get svc nginx
```

---

## 📂 Files in This Repository
- `nginx-deployment.yaml` — NGINX deployment configuration
- `nginx-service.yaml` — NGINX service configuration
- `single-node-setup.md` — Single node setup instructions
- `multi-node-setup.md` — Multi-node setup instructions

---

## ⚡ Usage Instructions
Clone and apply configs:
```bash
git clone https://github.com/Aishwarya-Nikam30/Kubernetes_Cluster_In_Detail.git
cd Kubernetes_Cluster_In_Detail/nginx
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

---

## 🤝 Contributing
Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📜 License
This project is open-source. Please check the repository for licensing details.
