# Lesson Four: Container Image Storage

OpenShift requires storage to be set aside for container images. This storage will house the container images used in the class. It’s important to realize that images are stored here but not the application data itself.

## Lab Instructions

Each of your OpenShift cluster machines has been provisioned with an additional 10G block device to host container images. 

1. From the workstation, ssh into master to verify the storage configured on it.
```
[student@workstation ~]$ sudo ssh master
Last login: Wed Dec  5 11:18:45 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]#
```

2. Verify the locally-attached storage of your machines. Run `lsblk` on individual hosts or via ansible. The following example was run from master:

```
[root@master ~]# lsblk
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
fd0                      2:0    1    4K  0 disk
sr0                     11:0    1 1024M  0 rom  
vda                    252:0    0   10G  0 disk
├─vda1                 252:1    0  500M  0 part /boot
└─vda2                 252:2    0  9.5G  0 part
  ├─rhel_pwob--r7-swap 253:0    0    1G  0 lvm  [SWAP]
  └─rhel_pwob--r7-root 253:1    0  8.5G  0 lvm  /
vdb                    252:16   0   10G  0 disk
[root@master ~]#
```

As shown by `lsblk`, `/dev/vdb` is 10G. We will use `/dev/vdb` as our container image block device. A good practice is to ensure there isn’t any previous information left on block devices. `wipefs` can be used to remove any leftover file system, raid, or other metadata from block devices.

3. Log out of master to return to the workstation.
```
[root@master ~]# exit
```

2. Run the following from your workstation host to prepare the block devices :
```
[student@workstation ~]$ sudo ansible all -m shell -a 'wipefs --all /dev/vdb'
```

3. After the storage is deemed suitable, run the following from your workstation:
```
[student@workstation ~]$ for i in master node1 node2; do echo $i; sudo ssh $i "cat << 'EOF' > /etc/sysconfig/docker-storage-setup
DEVS=/dev/vdb
VG=docker-vg
EOF
"; done
```

4. Run `docker-storage-setup`, which controls how container image data is stored (which we specified above), and ensure the docker daemon is running and enabled:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'docker-storage-setup'
[student@workstation ~]$ sudo ansible all -m shell -a 'systemctl enable docker && service docker start'
```
Ensure there are no errors before continuing.

[Lesson Five: Creating the OpenShift Ansible Inventory](05-lesson-create_inventory.md)
