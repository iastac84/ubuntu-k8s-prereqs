# ubuntu-k8s-prereqs

## Purpose

Quickly setup Ubuntu servers for Kubernetes 

Playbook based on steps from A Cloud Guru : https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1658326656460-Building%20a%20Kubernetes%20Cluster.pdf 

First ssh via public IPs, set passwords, set hostnames (sudo hostnamectl set-hostname <hostname>), copy ssh keys. 
Note, if Ubuntu was installed from scratch, ensure SSH is enabled and not firewalled. 

## Roles
- k8s-prereqs
- k8s-prereqs-ec2
- k8s-prereqs-pi

### `k8s-prereqs` role 

> Setup the Ubuntu servers

- Load kernel modules
- Modify system settings
- Install and configure containerd
- Disable swap 
- Install kubeadm, kubelet, and kubectl

### `k8s-prereqs-ec2` role

> Test / Placeholder for EC2 specfic requirements 

- Create test file 

### `k8s-prereqs-pi` role

> Specific Raspberry Pi requirements

- As Raspberry Pi does not use GRUB, allow systemd to act as the cgroups manager
- Updating /boot/firmware/cmdline.txt 

## Setup
First ssh via public IPs, set passwords, set hostnames (sudo hostnamectl set-hostname <hostname>), copy ssh keys 

### With examples

Update hardcodes values, including IP addresses and hostnames in:
- k8s-inv
- roles/k8s-prereqs/files/hosts 

### Usage

First update the `hosts` file to target your servers, and then test ansible: 

```shell
ansible all -i k8s-inv -m ping
ansible all -i k8s-inv -m setup 
ansible all -i k8s-inv -m setup  -a "filter=ansible_os_family"
```

Run the playbook with: 

```shell
ansible-playbook -i k8s-inv k8s-prereqs.yaml  -b --ask-become-pass
```

## TODO

- [] Tidy Up
- [] Update for rpm based distros 

