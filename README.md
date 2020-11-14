# vagrant-cephfs
<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Vagrant.png/394px-Vagrant.png" width="100" />&#09;<img src="https://upload.wikimedia.org/wikipedia/fr/b/b4/Ceph_Logo.png" width="80" />
</p>

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
Ansible will install cluster with [Cephadm](https://docs.ceph.com/en/latest/cephadm/install/).  
Cephadm install CephFS with containers, i chose to use [podman](https://podman.io/).  

### Debug

Comment out ``#ansible.verbose = "v"`` lines in [Vagrantfile](Vagrantfile).  
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
$ vagrant ssh ceph-1 -c "grep shell info.log"
	sudo /usr/sbin/cephadm shell --fsid ece971a4-267d-11eb-8dc7-5254002940ce -c /etc/ceph/ceph.conf -k /etc/ceph/ceph.client.admin.keyring
Connection to 192.168.121.28 closed.
```

### Use
To get status
```
$ vagrant ssh ceph-1 -c "sudo ceph status"
  cluster:
    id:     40d2a482-268f-11eb-9a42-52540047df21
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum ceph-1,ceph-2,ceph-3 (age 18m)
    mgr: ceph-1.rhaonx(active, since 20m), standbys: ceph-3.hydaeu
    osd: 9 osds: 9 up (since 15m), 9 in (since 15m)
 
  data:
    pools:   1 pools, 1 pgs
    objects: 0 objects, 0 B
    usage:   9.1 GiB used, 36 GiB / 45 GiB avail
    pgs:     1 active+clean
 
Connection to 192.168.121.172 closed.
```

### Clean
To remove everything
```
vagrant destroy -f
```
