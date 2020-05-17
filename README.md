
# Intro

Kubeadm-ansible project goal is to simple and fast installation of K8s cluster with high availability (HA) control plane without external loadbalancer.

Kubeadm-ansible use kubeadm installer to install K8s cluster and nginx to create high availbility (HA) control plane.

Kubeadm-ansible allow for setting following options:
- version of K8s cluster 
- version of nginx docker image
- pods subnet
- mount extra disk to masters and node 

Current version support Ubuntu 18.04 as base OS for masters and nodes.


# K8s cluster installation procedure

Check connectivity to all hosts:
```
ansible all -u root -i hosts -m ping
```

Install base packages on all hosts:
```
ansible-playbook -f 6 -v install-base.yaml -u root -i hosts
```

Init k8s cluster (control plane first master):
```
ansible-playbook -v init-cluster.yaml -u root -i hosts
```

Add masters to control plane:
```
ansible-playbook -v add-master.yaml -u root -i hosts -l kube-m2
ansible-playbook -v add-master.yaml -u root -i hosts -l kube-m3
```

Add nodes to k8s cluster:
```
ansible-playbook -f 3 -v add-node.yaml -u root -i hosts
```

