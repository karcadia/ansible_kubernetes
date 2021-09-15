# Build a basic Kubernetes stack. 
Deploy multi-node Kubernetes stack with Ansible. 

## Todo list
* Initial release only works with debian.
* Initial release only works with flannel or calico.
* Initial release only works with docker or cri-o.
* Customizing your pod network doesn't work yet.
* Will break into roles as appropriate probably along OS family lines.
* Linting.

## Ready
* Update the provided inventory file to match your environment.
```
[masters]
kube07.mccormicom.com

[workers]
kube08.mccormicom.com
kube09.mccormicom.com
```
* SSH Keys and Host Keys should already be configured per normal Ansible requirements.
* DNS entries will need to be in place for the kubernetes nodes.
* The /etc/hosts file will need to be sane with the hostname of the node referenced.
* SWAP will be destroyed if detected but the space wont be reclaimed.

## Set
* Update the variables in the top of the playbook if required.
```
---
- hosts: all
  vars:
    container_runtime: cri-o 
    kube_dns_domain: kube.mccormicom.com
    cni_plugin: calico
    pod_network_cidr: 10.42.0.0/16
    install_basic_tools: yes
    kubeadm_reset: no
```

## Go
Run `ansible-playbook -i inventory deploy_kube.yaml` from your jumpbox.
