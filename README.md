# cephfs-learning-lab
Automates local learning infrastructure installation for CephFS

# Setup
- Vagrant with libvirt provider
- 3G RAM
- 4 cores
- 20G disk free space.  
For archlinux:  
```
pacman -S vagrant libvirt qemu virt-manager
vagrant plugin install vagrant-libvirt
# choose libvirt in menu
```
Following [OS recommendations](https://docs.ceph.com/en/latest/start/os-recommendations/), lets use Debian Buster to test CephFS Octopus (15.2.z).  

```
# Download Debian 10
vagrant box add generic/debian10
```

# VMs

```
vagrant up
vagrant ssh ceph-1
$ ping -c 1 ceph-2
```
