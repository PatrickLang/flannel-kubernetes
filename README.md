# Ansible playbooks to deploy Kubernetes

The ansible playbooks are able to deploy a kubernetes cluster with
Windows minion nodes and Flannel as SDN solution.

## Ansible requirements

Minimum required ansible version is `2.4.2.0`. The recommended version is `2.7.2`.

For Linux: Make sure that you are able to SSH into the target nodes without being
asked for the password. You can read more [here](http://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html).

For Windows: Follow [this guide](https://docs.ansible.com/ansible/devel/user_guide/windows_setup.html)
to setup the node to be used with ansible.

#### Verifying the setup

To verify the setup and that ansible has been successfully configured you can run the following:

```
ansible -m setup all
```

This will connect to the target hosts and will gather host facts.
If the command succeeds and everything is green, you're good to go with running the playbook.

## How to use

Make sure to update first the [inventory](/contrib/inventory) with
details about the nodes.

To start the playbook, please run the following:
```
ansible-playbook kubernetes-cluster.yml
```

Currently supported Linux nodes:
- 16.04

Currently supported Windows nodes:
- Windows Server 2019 LTSC and build version 1809 (OS Version 10.0.17763.0)

### Running with Vagrant

1. If using Hyper-V, make sure you have an external switch created
1. `vagrant plugin install vagrant-hostmanager`
1. Build the `WindowsServer2019Docker` box using the Packer scripts - [build_windows_2019_docker.ps1](https://github.com/PatrickLang/packer-windows/blob/updated-eval/build_windows_2019_docker.ps1)
1. `vagrant box add --name WindowsServer2019Docker windows_2019_docker_hyperv.box`
1. `vagrant up`
  - If using Hyper-V, choose the external switch when prompted

## Ports that have to be opened on public clouds when using the playbooks

The following ports need to be opened if we access the cluster machines via the public address.

#### Kubernetes ports

- Kubernetes service ports (deployment specific): UDP and TCP `30000 - 32767`
- Kubelet (default port): TCP `10250`
- Kubernetes API: TCP `8080` for HTTP and TCP `443` for HTTPS

#### Ansible related ports

- WinRM via HTTPS: TCP `5986` (for HTTP also TCP `5985`)
- SSH: TCP `22`

### Further useful ports/types

- Windows RDP Port: 3389 (TCP)
- ICMP: useful for debugging

## Work in progress

- Flannel with network-mode overlay

- Different Linux versions support (currently only 16.04 supported)

### Known issues ( TO BE UPDATED )

