# cephfs-learning-lab
Automates local learning infrastructure installation for [CephFS](https://docs.ceph.com/en/latest/)

# Setup

VM provisionning with 3 nodes and 3 storage disks by node.  

### Prerequistes
- Vagrant with libvirt provider
- 3G RAM
- 4 cores
- 20G disk free space.  

### Install
For archlinux:  
```
pacman -S vagrant libvirt qemu virt-manager
vagrant plugin install vagrant-libvirt
# choose libvirt in menu
```
Following [OS recommendations](https://docs.ceph.com/en/latest/start/os-recommendations/), lets use Debian Buster to test CephFS Octopus (15.2.z).  

### Configure
You can edit [config](config) file.
```
BOX_IMAGE = "generic/debian10"
NODE_COUNT = 3
DISK_COUNT = 3
DISK_SIZE = "1G"
RAM = 1024
CPU = 1
HOSTNAME_PREFIX = "ceph"
```

### Provision

To start VMs use
```
vagrant up
```
It will provision VMs then use ansible to install and configure Ceph.  

### Install

Ansible will install cluster with [Cephadm](https://docs.ceph.com/en/latest/cephadm/install/).  
Cephadm install CephFS with containers, i chose to use [podman](https://podman.io/).
To run ansible playbook [playbook.yml](playbook.yml) manually, use:
```
vagrant provision
```

### Test
```
vagrant ssh ceph-1
$ ping -c 1 ceph-2
```

### Clean

To remove everything
```
vagrant destroy -f
```
