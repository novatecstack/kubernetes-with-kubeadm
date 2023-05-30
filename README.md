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
   3. RKE (Rancher Kubernetes Engines)

![image](https://github.com/novatecstack/kubernetes-with-kubeadm/assets/121426292/0b1a05d5-add2-44db-aa52-e2d6dffc0cb0)

## Setup a Kubernetes cluster (master-worker) using kubeadm
   We will consider building a Kubernetes setup with <b>one Master node</b> and <b>two Worker nodes</b>.
   Here, we will discuss in detail the process of deploying a minimal Kubernetes cluster on CentOS 7 servers. This installation is for a single control-plane cluster.
</br> 

Below are the steps we'll perform in order to setup K8s cluster:
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

### Step 1: Prepare Kubernetes Servers
The minimal server requirements for the servers used in the cluster are:
- 2 GiB or above RAM per machine–any less leaves little room for your apps
- 2 CPUs on the control-plane node
- Network connectivity between all the machines in K8s cluster

The below setup is for development purpose only:
| Server Type  | Server Hostname | Specification |
|     :---:    |     :---:      |     :---:     |
| Master  | k8s-master01.novatec.com	| 2 CPU, 4GB RAM |
| Worker  | k8s-master01.novatec.com	| 2 CPU, 4GB RAM |
| Worker  | k8s-master01.novatec.com	| 2 CPU, 4GB RAM |

Login to all the
### Step 2: Install kubelet, kubeadm and kubectl

### Step 3: Disable SELinux and Swap
### Step 4: Install Container runtime
### Step 5: Configure Firewalld
### Step 6: Initialize your control-plane node (Master)
### Step 7: Install network plugin
### Step 8: Add worker nodes to the K8s cluster
### Step 9: Deploy application on cluster
### Step 10: Install Kubernetes Dashboard (Optional)
### Step 11: Install an Ingress Controller
### Step 12: Configure Persistent volume storage
