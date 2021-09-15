# Build a basic Kubernetes stack. 
Deploy multi-node Kubernetes stack with Ansible. 

## Todo list
* Initial release only works with debian.
* Initial release only works with flannel.
* Initial release only works with docker.
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
* SSH Keys and Host Keys should already be configured, just like any other Ansible playbook would require.
* DNS entries will need to be in place for the kubernetes nodes.
* The /etc/hosts file will need to be sane with the hostname of the node referenced.

## Set
* Update the variables in the top of the playbook if required.
```
---
- hosts: all
  vars:
    container_runtime: docker
    kube_dns_domain: kube.mccormicom.com
    cni_plugin: flannel
    pod_network_cidr: 10.42.0.0/16
    install_basic_tools: yes
```

## Go
Run `ansible-playbook -i inventory deploy_kube.yaml` from your jumpbox.

## Next steps
* Optional: Add a static route for your pod and service networks.
