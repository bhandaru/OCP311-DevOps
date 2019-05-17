# Lesson Two: Prep the workstation 
# Understanding Subscriptions and Software Repositories

OpenShift is built on top of Red Hat Enterprise Linux. The installation procedure adds additional software repositories and leverages an ansible inventory file as a centralized place to configure and automate the installation of OpenShift across multiple machines.

Because Red Hat software is made available via subscriptions, a common production task is to target subscriptions to exact machines. A common workflow is to register a host, select the correct subscription and repositories, and install or update update software as needed. Solutions like Red Hat Satellite can automate the registration and maintenance of Red Hat software. 

We have pre-arranged the subscriptions used in this class, so we will not be registering our machines. We are using yum-config-manager today however production environments would leverage subscription-manager to locally administer subscriptions and software repositories. yum-config-manager will approximate the process of adding the correct subscriptions to an OpenShift deployment.

## Lab Instructions
Our first step is to ensure we have the proper software repositories enabled. We start by disabling any existing repositories and adding only the repositories we need. 

1. On your workstation machine, execute the following to become a root user:
```
[student@workstation ~]$ sudo -i
```

2. On your workstation clone the git repo ```https://github.com/xtophd/OCP-Workshop.git```
```
[root@workstation ~]$ git clone https://github.com/xtophd/OCP-Workshop.git
Cloning into 'OCP-Workshop'...
remote: Enumerating objects: 81, done.
remote: Counting objects: 100% (81/81), done.
remote: Compressing objects: 100% (81/81), done.
remote: Total 6659 (delta 39), reused 0 (delta 0), pack-reused 6578
Receiving objects: 100% (6659/6659), 7.46 MiB | 13.47 MiB/s, done.
Resolving deltas: 100% (4076/4076), done.
```
3. Change current working directory to ```OCPWorkshop```

```
[root@workstation ~]$ cd OCPWorkshop
[root@workstation OCPWorkshop]$
```
4. Inspect the contents of the git repo.
```
[root@workstation OCP-Workshop]# ls -la
total 64
drwxr-xr-x. 8 root root  4096 May 13 23:36 .
dr-xr-x---. 9 root root  4096 May 14 19:39 ..
drwxr-xr-x. 2 root root   103 May 15 00:25 configs
drwxr-xr-x. 4 root root    58 May 13 23:36 documentation
drwxr-xr-x. 8 root root  4096 May 13 23:36 .git
-rw-r--r--. 1 root root   458 May 13 23:36 install-docker.sh
-rw-r--r--. 1 root root  1883 May 13 23:36 install-openshift.sh
-rw-r--r--. 1 root root 35147 May 13 23:36 LICENSE
drwxr-xr-x. 3 root root  4096 May 15 00:12 playbooks
-rw-r--r--. 1 root root  1490 May 13 23:36 README.adoc
drwxr-xr-x. 2 root root    25 May 13 23:36 screenrc
drwxr-xr-x. 5 root root    56 May 13 23:36 src
```

5. Examine the contents ansible playbook for preparing the workstation.  
```
[root@workstation OCP-Workshop]# vi playbooks/ocp-3.11-prep-workstation.yml
```

6. Ensure that ```openshift-ansible``` is being installed as part of the playbook on the last task. If it is not in the list of tasks then add it.
```
    - name: "YUM install atomic-openshift-clients plus misc required packages"
      yum:
        name=atomic-openshift-clients,openshift-ansible,wget,git,net-tools,bind-utils,yum-utils,iptables-services,bridge-utils,bash-completion,kexec-tools,sos,psacct,nfs-utils,nfs4-acl-tools,lynx,openshift-ansible
        state=installed
```
7. Save and exit the file to return to current working directory.
```
[root@workstation OCP-Workshop]#
```

[Lesson Three: Create OpenShift Inventory file](03-lesson-create_inventory.md)
