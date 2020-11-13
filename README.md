# cephfs-learning-lab
Automates local learning infrastructure installation for CephFS

# Setup

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

# VMs

```
vagrant up
vagrant ssh ceph-1
$ ping -c 1 ceph-2
```
