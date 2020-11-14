# vagrant-cephfs
Automates local learning infrastructure installation for [CephFS](https://docs.ceph.com/en/latest/)

# Setup

VM provisionning with 3 nodes and 3 storage disks by node.  

### Prerequistes
- Vagrant with libvirt provider
- 3G RAM
- 4 cores
- 20G disk free space.  

### Prepare
For archlinux:  
```
sudo pacman -S vagrant libvirt qemu virt-manager
sudo gpasswd -a $USER libvirt
systemctl start libvirtd
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
# Cephfs requires 5G minimum disks
DISK_SIZE = "5G"
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
To debug  
```
vagrant up --no-destroy-on-error
# if it fails, fix and rerun ansible with
vagrant provision
```

### Access

##### WebUI
To find how to access to dashboard, get ``User`` and ``Password`` on the first node log  
```
$ vagrant ssh ceph-1 -c "grep -B1 Password info.log"
	   User: admin
	Password: cgib0cyibd
Connection to 192.168.121.37 closed.
```
Then access to https://192.168.121.37:8443

##### Shell
```
vagrant ssh ceph-1 -c "grep shell info.log"
```

### Clean
To remove everything
```
vagrant destroy -f
```
