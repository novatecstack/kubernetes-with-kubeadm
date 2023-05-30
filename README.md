# Creating a Kubernetes cluster with kubeadm
  - Kubeadm is a tool used to build Kubernetes (K8s) clusters. 
  - Kubeadm performs the actions necessary to get a minimum viable cluster up and running quickly. 
  - By design, it cares only about bootstrapping, not about provisioning machines (underlying worker and master nodes). 
  - Kubeadm also serves as a building block for higher-level and more tailored tooling.
  - We can use kubeadm for creating a production-grade Kubernetes environment.
  - <b>kubeadm</b> also supports other cluster lifecycle functions, such as bootstrap tokens and cluster upgrades

## Installation Architecture
   1. <b>Single node cluster</b>: You use one server to install the control plane and configure it to also accept workloads.
   2. <b>Single controller, multi worker</b>: You configure one server as the single controller node of the cluster and add additional worker nodes that host the workloads. The minimum required hardware spec for the nodes is at least 2 CPU and 2GB RAM for controller nodes, and 1 CPU, 2GB RAM for worker nodes.
   3. <b>Multi controller, multi worker</b>: This architecture adds additional controller nodes for a more robust cluster. The number of controller nodes should confirm to the equation of 2*n + 1 to allow a quorum in the case that a controller node goes down.

## kubeadmin Features
   1. Quick minimum K8s cluster creation
   2. Portable
   3. Local Development
   4. Building blocks for other tools

## Kubeadm vs kOps vs Kubespray
   kubeadm isn’t the only tool available to deploy a K8s cluster. kOps and Kubespray are two popular tools for the same general use case. However, each tool offers different functionality that makes them ideal for different applications.
   Before we dive into how to create the Kubeadm cluster, let's take a closer look at how it stacks up to the alternatives:
   1. kOps
   2. kubeSpray

![image](https://github.com/novatecstack/kubernetes-with-kubeadm/assets/121426292/0b1a05d5-add2-44db-aa52-e2d6dffc0cb0)

## Setup a Kubernetes cluster (master-worker) using kubeadm
   We will consider building a Kubernetes setup with <b>one Master node</b> and <b>two Worker nodes</b>.
   Let us assume that we have three Ubuntu Linux machines named Master, xWorker01, and Worker02 in the same network. For practice purposes, you can create 3 VMS in VirtualBox or you can create 3 VMs in the cloud. The VMs will be accessible from each other. We will add the necessary configuration in the master machine to make it a Kubernetes master node, and connect the worker1 and worker2 to it.

- Step 1: Prepare Kubernetes Servers
- Step 2: Install kubelet, kubeadm and kubectl
- Step 3: Disable SELinux and Swap
- Step 4: Install Container runtime
- Step 5: Configure Firewalld
- Step 6: Initialize your control-plane node (Master)
- Step 7: Install network plugin
- Step 8: Add worker nodes to the K8s cluster
- Step 9: Deploy application on cluster
- Step 10: Install Kubernetes Dashboard (Optional)
- Step 11: Install an Ingress Controller
- Step 12: Configure Persistent volume storage

### Step-01: Installing Docker as the container runtime Interface on the three Virtual Machines (CentOS)
```
#removing existing docker
sudo yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-engine
```
```
#or simply run this command to remove all the docker specific packages
sudo yum remove docker*
```

```
#Adding a docker repository
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```
#Install Docker engine
sudo yum install docker-ce docker-ce-cli containerd.iosystemctl enable docker -y
```

```
#Start and automate docker to start at run time
sudo systemctl start docker
sudo systemctl enable docker
```

```
#verify docker installation
docker container ls
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS               NAMES
```

Kubeadm will by default use docker as the container runtime interface. In case a machine has both docker and other container runtimes like <b>contained, docker takes precedence</b>. If you don't specify a container runtime interface, <b>kubeadm</b> will automatically search for the installed CRI by scanning through the default Linux domain sockets.

    
### Step-02: Installing kubeadm tool (CentOS)
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
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

```
#Set SELinux in permissive mode (effectively disabling it)

setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

```
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```

```
systemctl enable --now kubelet
```

### Step-03: Initializing the control plane (making the node as) <b>Master</b>
<b>kubeadm init</b> will initialize this machine to make it as master. <b>kubeadm init</b> first runs a series of prechecks to ensure that the machine is ready to run Kubernetes.</br></br>
These prechecks expose warnings and exit on errors. kubeadm init then downloads and installs the cluster control plane components. This may take several minutes.</br></br>
We have to take care that the Pod network must not overlap with any of the host networks: you are likely to see problems if there is any overlap. We will specify the private CIDR for the pods to be created.
```
$ kubeadm init --pod-network-cidr 10.15.0.0/16
```
</br>In case you are not creating the production environment and your master machine has only one CPU (minimum recommended CPU is 2), if an error occurs due to preflight check of CPU, then run the below command:
```
#swapoff -a (if swap issue is seen - only in testing or practice)
kubeadm init --ignore-preflight-errors=NumCPU --pod-network-cidr 10.15.0.0/16
```

Now, in order to use the K8s cluster, we need to run the below commands as a normal user:
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

and if you're a root user, run:
```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

<b> Now the machine is initialized as master. </b></br>
At this stage, if you will check the list of nodes in the K8s cluster, we will see only the master node:
```
kubectl get nodes
```


### Step-04: Installing a Pod network add-on
