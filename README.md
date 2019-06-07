# kubernetes-hello-world

## Steps

As root user

```bash
setenforce 0
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
```

Setup firewall rules
```bash
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10251/tcp
firewall-cmd --permanent --add-port=10252/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload
modprobe br_netfilter
echo '1' /proc/sys/net/bridge/bridge-nf-call-iptables
```

Kubernetes repo
```text
# /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
       https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
```

Install Docker and Kubernetes
```bash
yum install kubeadm docker -y

systemctl restart docker && systemctl enable docker
systemctl restart kubelet && systemctl enable kubelet
```


Turn off swap
```bash
swapoff -a
```

Initialize using kubeadm
```bash
kubeadm init

mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# Set up DNS
export kubever=$(kubectl version | base64 | tr -d '\n')
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
```

Join commands

```bash
# Original
kubeadm join 10.0.2.15:6443 --token e10edi.hp3rblqxcy74wbmi \
    --discovery-token-ca-cert-hash sha256:8281eebb9e81aa7a4d2a34dc18509ce7bce88cd85cfff89557681737ceae8878

# Modified
kubeadm join 172.17.8.101:6443 --token c55lrg.nyakut1fogrlu0t0 --discovery-token-ca-cert-hash sha256:cee896a67ba66459d61aca3b7a3dcf33a231fc206d02ab23c87432f71b648fb6
```

node.vm.network "forwarded_port", guest: 6443, host: 6443 + i, protocol: "tcp"


```bash
sudo systemctl stop nfs-kernel-server.service
sudo systemctl disable nfs-kernel-server.service
sudo systemctl enable nfs-kernel-server.service
sudo systemctl start nfs-kernel-server.service
```
