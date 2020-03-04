## What is it?
This is a bunch of scripts to fire up small Kubernetes cluster with Calico or Flannel networking.

Suggested environment:
* virtual machine (VirtualBox or something like)
* Ubuntu 18.04

Each virtual machine is implied to have two network adapters:
* NAT (for Internet access)
* Host-only adapter (to communicate betweed nodes and host machine)

This is not a requirement! If a virtual machine could have Internet access and LAN on the same adapter (NAT Network in VirtualBox), this would be fine.

## Creating cluster

1. Install required packages and hold versions:

`sudo ./install`

2. Create the cluster itself. As the only argument we need is interface name (usually Host-only interface):
* Kubernetes API server will listen on it
* node to node communication will happen through it

To see interface names just start it without parameters:

`sudo ./init`

To create cluster run it with interface name parameter:

`sudo ./init enp0s8`

**Script will automatically copy kubectl config file to `~/.kube/config`. If you have any important configuration there, please, back it up first.**

3. Cluster creation script will prepare all required manifests for network plugins. All you have to do is to select which one you like more:

### Flannel

`kubectl apply -f network/flannel.yml`

### Calico

`kubectl apply -f network/calico.yml`

## Joining a node

1. Install required packages and hold versions:

`sudo ./install`

2. To join a node you have to run `./join` with two parameters:
* name of the interface for node to node communication
* endpoint of Kubernetes API server

To see interface names start it without parameters:

`sudo ./join`

To join just start it with parameters:

`sudo ./join enp0s8 192.168.56.110:6443`
