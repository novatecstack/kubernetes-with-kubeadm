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
