# Lesson Five: Creating the OpenShift Ansible Inventory

OpenShift uses a centralized Ansible inventory file to describe, install, and modify the environment. This file details the machines used by OpenShift, their roles, what features are enabled, and many other details. The inventory file is crucial for installation and is also utilized in running environments to make changes. One common task is to add or remove machines after the installation of the environment has been completed.

OpenShift requires access to DNS. Crucially, OpenShift requires the ability to utilize DNS wildcards. This is required for arbitrary applications to be created and reachable via DNS names. 

Our environment has pre-configured a wildcard DNS domain. 

## Lab Instructions

1. On any machine, ping arbitrary DNS names on `cloud.example.com` to refer back to OpenShift’s master:
```
[student@workstation ~]$ ping jobu.cloud.example.com
PING jobu.cloud.example.com (10.0.0.11) 56(84) bytes of data.
64 bytes from node1.example.com (10.0.0.11): icmp_seq=1 ttl=64 time=1.32 ms
64 bytes from node1.example.com (10.0.0.11): icmp_seq=2 ttl=64 time=0.536 ms
^C

[root@master ~]# ping zappa.cloud.example.com
PING zappa.cloud.example.com (10.0.0.11) 56(84) bytes of data.
64 bytes from node1.example.com (10.0.0.11): icmp_seq=1 ttl=64 time=0.665 ms
64 bytes from node1.example.com (10.0.0.11): icmp_seq=2 ttl=64 time=0.516 ms
^C
```
It is important to have a DNS wildcard domain implemented well in advance of OpenShift’s installation. This domain is reflected as `openshift_master_default_subdomain` in the inventory file.

OpenShift’s installation documentation provides examples of the inventory file. You can also find examples under `/usr/share/doc/openshift-ansible-docs-$version/docs/example-inventories/`.

**Important note:** Our OpenShift cluster will communicate via private, non-routable IP addresses within the training environment. In order to directly access OpenShift from your browser, it will be necessary to know the public IP address of the master machine. This IP address can be found on the page listing your student information, which your instructor has provided.

2. To access the public IP address of the master host, go to the student page and click `Control Center` (Just below your Student number). This link gives you additional access to the training environment. Find the box for `MASTER` and take note of the IP address following `web: `. You will add this IP address to the inventory below.

3. The inventory file resides on the master host. Log onto the master host:
```
[student@workstation ~]$ sudo ssh master
Last login: Thu Aug  2 10:39:17 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]#
```

4. There should be a default, completely commented `/etc/ansible/hosts` file on master. Blanking this file is a good practice before creating your OpenShift inventory. Run the folling on master to blank the default inventory file:
```
[root@master ~]# > /etc/ansible/hosts
```

5. Now build the inventory file used by OpenShift. Replace the instances of **YOUR_MASTER_PUBLIC_IP_ADDRESS** with the public IP address of your master host, explained above. Open a text editor and edit the ansible hosts file:  
```
[root@master ~]# vim /etc/ansible/hosts
```
Copy and paste the following content into `/etc/ansiible/hosts` file. Ensure you have replaced the **YOUR_MASTER_PUBLIC_IP_ADDRESS** with your cluster's master node's public IP address. 

:information_source: **_Note that you have to replace it in 3 places._**

```
# 20180821 -- begin OpenShift inventory file
# Create an OSEv3 group that contains the masters, nodes, and etcd groups
[OSEv3:children]
masters
nodes
etcd

# Set variables common for all OSEv3 hosts
[OSEv3:vars]
# SSH user, this user should allow ssh based auth without requiring a password
ansible_ssh_user=root
os_firewall_use_firewalld=True

# 20180925
openshift_master_default_subdomain=YOUR_MASTER_PUBLIC_IP_ADDRESS.xip.io

# 20180521, from mseida, we are *way* under-resourced
openshift_disable_check=memory_availability,disk_availability

openshift_deployment_type=openshift-enterprise
openshift_release=v3.10

# uncomment the following to enable htpasswd authentication; defaults to DenyAllPasswordIdentityProvider
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]

# host group for masters
[masters]
master.example.com openshift_public_hostname=YOUR_MASTER_PUBLIC_IP_ADDRESS.xip.io openshift_hostname=master.example.com openshift_ip=10.0.0.11 openshift_public_ip=YOUR_MASTER_PUBLIC_IP_ADDRESS

# host group for etcd
[etcd]
master.example.com

# host group for nodes, includes region info
[nodes]
master.example.com openshift_node_group_name='node-config-master-infra'
node1.example.com openshift_node_group_name='node-config-compute'
node2.example.com openshift_node_group_name='node-config-compute'

# end OpenShift inventory
```

Please carefully examine `/etc/ansible/hosts` to ensure everything appears correct.

At this point, you are ready to validate our setup and install OpenShift.

[Lesson Six: Installing the OpenShift Cluster](06-lesson-install_cluster.md)
