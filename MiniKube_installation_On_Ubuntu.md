# Smooth-Installation-of-Minikube-On-VM

NOTE: Following Commands are Only for Ubuntu Linux but After some minor modification You can Use it for CentOS AmazonLinux and other linux OS. 

#Run the Following Commands To Install MiniKube On Ubuntu Server

sudo apt update && apt -y install docker.io

apt-get update
apt-get install -y conntrack socat selinux-utils ebtables ethtool

#!/usr/bin/env bash
# run as root
apt-get update
apt-get install -y conntrack socat selinux-utils ebtables ethtool

# turn off swap
swapoff -a

KUBECTL_VERSION=v1.20.1
MINIKUBE_VERSION=v1.20.0

# get kubectl
curl -LO https://storage.googleapis.com/minikube/releases/$MINIKUBE_VERSION/minikube-linux-amd64
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl

# get minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/$MINIKUBE_VERSION/minikube-linux-amd64
chmod +x minikube
mv minikube /usr/local/bin/

# start minikube
minikube start \
--vm-driver=none \
--kubernetes-version=$KUBECTL_VERSION \
--extra-config=controller-manager.node-cidr-mask-size=16 \
--extra-config=controller-manager.allocate-node-cidrs=true \
--extra-config=controller-manager.cluster-cidr=10.244.0.0/16 \
--extra-config=apiserver.authorization-mode=Node,RBAC \
--extra-config=apiserver.service-account-signing-key-file=/var/lib/minikube/certs/sa.key \
--extra-config=apiserver.service-account-issuer=kubernetes.default.svc \
--extra-config=kubeadm.ignore-preflight-errors=SystemVerification \
--extra-config=kubeadm.pod-network-cidr=10.244.0.0/16 \
--extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
