# Lesson Six: Installing the OpenShift Cluster

With DNS verified, prerequisite packages installed, and the inventory file in place, we are ready to perform the OpenShift installation. The installation is contained within two steps. The first step completes prerequisites prior to installation, and the second step performs the actual installation. The first step could take as little as five minutes, while the install can take thirty or more minutes.

## Lab Instructions

These steps will be performed on the master virtual machine. 

1. Log in now to master:
```
[student@workstation ~]$ sudo ssh master
Last login: Thu Aug  2 10:39:23 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]#
```
If this is the first time you've tried to log onto other hosts from master, you'll be prompted to accept fingerprints. We can avoid being prompted to access fingerprints by running the following:
```
[root@master ~]# export ANSIBLE_HOST_KEY_CHECKING=False
```

2. Onward to installation. Become root on the master (if needed) and run the following:
```
[root@master ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[root@master ~]# hostname
master.example.com
```

3. Run the following command to execute the ansible playbook to install the prerequisites.
```
[root@master ~]# time ansible-playbook \
    /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
```

4. Run the following command to execute the ansible playbook to install OpenShift cluster.
```
[root@master ~]# time ansible-playbook \
    /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
```

This takes about thirty minutes to complete. At the end of it, the output should be as below:
```
...

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************************************************
localhost                  : ok=14   changed=0    unreachable=0    failed=0   
master.example.com         : ok=628  changed=276  unreachable=0    failed=0   
node1.example.com          : ok=120  changed=56   unreachable=0    failed=0   
node2.example.com          : ok=120  changed=56   unreachable=0    failed=0   


INSTALLER STATUS *****************************************************************************************************************************************************************************************************************************************************************
Initialization              : Complete (0:00:32)
Health Check                : Complete (0:00:36)
Node Bootstrap Preparation  : Complete (0:06:12)
etcd Install                : Complete (0:01:07)
Master Install              : Complete (0:05:26)
Master Additional Install   : Complete (0:02:24)
Node Join                   : Complete (0:04:25)
Hosted Install              : Complete (0:01:45)
Web Console Install         : Complete (0:00:57)
Service Catalog Install     : Complete (0:03:32)

real    27m16.016s
user    5m53.063s
sys    3m50.009s
You have new mail in /var/spool/mail/root
[root@master ~]#
```
Errors occurring during the installation will appear in bright red. Provided the installation completed successfully, now proceed to test and interact with the environment.

[Lesson Seven: Interacting With Your Installation](07-lesson-interacting.md)
