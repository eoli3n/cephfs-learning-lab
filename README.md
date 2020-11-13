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

### Run

To start VMs use
```
vagrant up
```
It will provision VMs then use ansible to install and configure Ceph.  
To rerun ansible playbook [playbook.yml](playbook.yml)
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
