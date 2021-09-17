# Build a basic Kubernetes stack. 
Deploy multi-node Kubernetes stack with Ansible. Kept as basic and simple (and readable) as possible. 

## Currently supported
* OS Support:
  * Redhat family
  * Debian family
  * Arch Linux
* CNI Support: flannel or calico (weave coming soon)
* Container Runtime Support: docker or cri-o or containerd (containerd is broken on debian) 

## Todo list
* Linting.
* Test more Ansible versions beyond 2.9.
* Enforce either shortname or FQDN as node name for all OS families.
* Will break into roles as appropriate probably along OS family lines.
* Add actual firewall rule enforcement.
* Multi-master mostly works but full HA tolerance still doesn't seem to be working.
* Test weave on a few platforms.
* Fix containerd for debian
* Customizing your pod network doesn't fully work yet.

## Test log
* Arch + cri-o + weave = success on 09/17/2021
* Arch + cri-o + calico = success on 09/17/2021 
* Arch + docker = fail
* Arch + containerd = fail
* Debian + containerd = fail
* Debian + cri-o + calico =
* Debian + cri-o + flannel =
* Debian + docker + calico =
* Debian + docker + flannel =
* CentOS + cri-o + calico =
* CentOS + cri-o + flannel = 

## Ready
* Update the provided inventory file to match your environment.
```
[masters]
kube01.mccormicom.com
kube02.mccormicom.com

[workers]
kube03.mccormicom.com
kube04.mccormicom.com
```
* Ansible requires SSH Keys be configured, and host keys either added or ansible.cfg updated.
* DNS entries will need to be in place for the kubernetes nodes.
* DNS aliases or (ideally) a load balancer will need to be in place for multi-master.
* SWAP will be destroyed if detected but the space wont be reclaimed.
* Ansible requires python be installed, this is a manual step for Arch.

## Set
* Update the variables in the top of the playbook if required.
```
---
- hosts: all
  vars:
    container_runtime: cri-o
    kube_dns_domain: kube.mccormicom.com
    control_plane_endpoint: kube.mccormicom.com
    cni_plugin: calico
    pod_network_cidr: 10.244.0.0/16
    install_basic_tools: no
    kubeadm_reset: no
    flannel_manifest: "https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml"
    calico_manifest: "https://docs.projectcalico.org/manifests/calico.yaml"
    weave_manifest: "https://cloud.weave.works/k8s/v1.16/net.yaml"
```

## Go
Run `ansible-playbook -i inventory deploy_kube.yaml` from your jumpbox or workstation.
