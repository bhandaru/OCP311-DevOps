# Lesson Four: Installing the OpenShift Cluster

With DNS verified, prerequisite packages installed, and the inventory file in place, we are ready to perform the OpenShift installation. The installation is contained within two steps. The first step completes prerequisites prior to installation, and the second step performs the actual installation. The first step could take as little as five minutes, while the install can take thirty or more minutes.

## Lab Instructions

These steps will be performed on the master virtual machine. 

1. Log in now to workstation and become root. Ensure you are working directory is ```/root/OCP-Workshop```:
```
[student@workstation ~]$ sudo -i
Last login: Thu Aug  2 10:39:23 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@workstation ~]# cd OCP-Workshop
[root@workstation OCP-Workshop]#
```
If this is the first time you've tried to log onto other hosts from master, you'll be prompted to accept fingerprints. We can avoid being prompted to access fingerprints by running the following:
```
[root@workstation OCP-Workshop]# export ANSIBLE_HOST_KEY_CHECKING=False
```

2. Onward to installation. Become root on the master (if needed) and run the following:
```
[root@workstation OCP-Workshop]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[root@workstation OCP-Workshop]# hostname
workstation.example.com
```

3. Run the following command to execute the workstation prep playbook.
```
[root@workstation OCP-Workshop]# time ansible-playbook \
 -i ./configs/ocp-3.11-workshop -f 20 ./playbooks/ocp-3.11-prep-workstation.yml
```

You should see an output like below.
```
PLAY RECAP **************************************************************************************************************************************************************
workstation.example.com    : ok=6    changed=3    unreachable=0    failed=0   


real	0m42.450s
user	0m9.103s
sys	0m4.643s
```

4. Run the following command to execute the cluster prep playbook.
```
[root@workstation OCP-Workshop]# time ansible-playbook \
 -i ./configs/ocp-3.11-workshop -f 20 ./playbooks/ocp-3.11-prep-cluster.yml
```

You should see an output like below.
```
PLAY RECAP **************************************************************************************************************************************************************
master.example.com         : ok=11   changed=6    unreachable=0    failed=0   
node1.example.com          : ok=11   changed=6    unreachable=0    failed=0   
node2.example.com          : ok=11   changed=6    unreachable=0    failed=0   
node3.example.com          : ok=11   changed=6    unreachable=0    failed=0   


real	2m9.742s
user	0m42.495s
sys	    0m16.366s
```

5. Run the following command to execute the ansible playbook to install the prerequisites.
```
[root@master ~]# time ansible-playbook \
-i ./configs/ocp-3.11-workshop -f 20 /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
```

6. Run the following command to execute the ansible playbook to install OpenShift cluster.
```
[root@master ~]# time ansible-playbook \
-i ./configs/ocp-3.11-workshop -f 20 /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
```

This takes about 60 minutes to complete. At the end of it, the output should be as below:
```
PLAY RECAP **************************************************************************************************************************************************************
localhost                  : ok=12   changed=0    unreachable=0    failed=0   
master.example.com         : ok=885  changed=324  unreachable=0    failed=0   
node1.example.com          : ok=128  changed=36   unreachable=0    failed=0   
node2.example.com          : ok=127  changed=36   unreachable=0    failed=0   
node3.example.com          : ok=127  changed=36   unreachable=0    failed=0   


INSTALLER STATUS ********************************************************************************************************************************************************
Initialization                 : Complete (0:00:00)
Health Check                   : Complete (0:00:01)
etcd Install                   : Complete (0:00:00)
Node Bootstrap Preparation     : Complete (0:00:00)
Master Install                 : Complete (0:00:00)
Master Additional Install      : Complete (0:00:00)
Node Join                      : Complete (0:00:00)
Hosted Install                 : Complete (0:00:07)
Cluster Monitoring Operator    : Complete (0:00:01)
Web Console Install            : Complete (0:00:01)
Console Install                : Complete (0:00:01)
Metrics Install                : Complete (0:00:01)
metrics-server Install         : Complete (0:00:01)
Logging Install                : Complete (0:00:01)
Service Catalog Install        : Complete (0:00:01)
OLM Install                    : Complete (0:00:00)
Node Problem Detector Install  : Complete (0:00:00)


real	39m45.512s
user	14m31.588s
sys	5m41.406s

```
Errors occurring during the installation will appear in bright red. Provided the installation completed successfully, now proceed to test and interact with the environment.

[Lesson Five: Interacting With Your Installation](05-lesson-interacting.md)
