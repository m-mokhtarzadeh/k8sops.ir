---
date: 2022-12-11T15:26:15Z
lastmod: 2022-12-11T15:26:15Z
publishdate: 2022-12-11T15:26:15Z

title: Create Lab from scratch
description: Kubernetes Operations Fundamentals
images:
- images/kops.jpg
---

# Create Lab from scratch
### Step 1: Download oracle linux 8
Download oracle linux 8 from following repository:  
[Oracle Linux download repository](https://yum.oracle.com/oracle-linux-isos.html)

### Step 2: Install dependencies
Install following dependencies:
```bash
dnf install -y iproute-tc vim
```

### Step 3: Disable firewall
Disable firewall:  
```bash
systemctl stop firewalld
systemctl disable firewalld
systemctl mask firewalld
```

### Step 4: Disable selinux
Disable selinux:  
```bash
sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
```

### Step 5: Update system packages
Update system packages with following command:
```bash
dnf update
```

### Step 6: Install latest version of docker
Add docker repository to server package repository:
```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
Install docker packages:
```bash
sudo dnf install docker-ce docker-ce-cli containerd.io
```
Enable and start docker service with following commands:
```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

### Step 7: Pull images for required hands-on lab
```bash
docker pull hello-world
docker pull ubuntu
docker pull nginx
docker pull php:7-apache
```

### Step 8: Configuring Docker for kubernetes installation
```bash
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
```
```bash
systemctl restart docker
```

### Step 9: Configuring System for kubernetes installation
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
```

```bash
cat << EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
fs.file-max = 1500000
fs.nr_open = 1500000

vm.max_map_count = 262144
net.ipv4.ip_local_reserved_ports = 30000-32767

net.ipv4.ip_local_port_range = 1024 65535

net.netfilter.nf_conntrack_max = 32768000
vm.swappiness = 0
net.ipv4.ip_forward = 1
EOF
```

```bash
sysctl --system
```

Disable swap
```bash
swapoff -a
# remove swap from /etc/fstab
# make sure swap in lvm mode removed from grub
```

### Step 10: Install kubernetes
Add kubernetes repository to system repository:
```bash
cat << EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
```

Install kubernetes packages:
```bash
dnf install -y kubelet-1.23.15 kubeadm-1.23.15 kubectl-1.23.15 --disableexcludes=kubernetes
```

Enable kubernetes service:
```bash
systemctl enable kubelet
```

Pull kubernetes required container images:
```bash
kubeadm config images pull
docker pull docker.io/calico/kube-controllers:v3.24.5
docker pull docker.io/calico/node:v3.24.5
docker pull docker.io/calico/cni:v3.24.5
```

Download required manifest:
```bash
curl https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/calico.yaml -O
```