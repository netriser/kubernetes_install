# Kubernetes Install Role

An Ansible role to install and configure Kubernetes clusters, including master and worker nodes.

## Features
- Installs Kubernetes components (kubeadm, kubelet, kubectl).
- Configures Kubernetes master and worker nodes.
- Initializes the Kubernetes cluster and sets up networking calico.

## Requirements
- Ansible 2.9 or higher.
- Supported platforms:
  - **Debian-based systems:** Ubuntu, Debian.
- `containerd` will be installed on all nodes.

## Role Variables
The following variables can be customized for this role:

### Defaults
Located in `defaults/main.yml`:
```yaml
kubernetes_prerequisites_packages:
  - ca-certificates
  - bridge-utils
kubernetes_minor_version: "1.30"
kubernetes_version: "{{ kubernetes_minor_version }}.3-1.1"
kubernetes_cidr_pods: "10.200.0.0/16"
# I used Aws tags, you can pute your first master here
master_hostname: "{{ groups['tag_Name_k8s_master_1'][0] }}"
```


Exemple playbook:
```yaml
---
- name: Install Kubernetes cluster first Master
  hosts: tag_Name_k8s_master_1
  become: yes
  gather_facts: yes
  vars:
    kubernetes_minor_version: "1.30"
    kubernetes_version_extention: "1-1.1"
    
  roles:
    - kubernetes_install

- name: Install Kubernetes cluster other masters and workers
  hosts: tag_app_k8s_*,!tag_Name_k8s_master_1
  become: yes
  gather_facts: yes
  vars:
    kubernetes_minor_version: "1.30"
    kubernetes_version_extention: "1-1.1"
    
  roles:
    - kubernetes_install
```
## Dependencies
None.

## License
MIT

## Author Information
This role was created by Chakib. Contributions and feedback are welcome!
