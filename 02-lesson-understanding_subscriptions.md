# Lesson Two: Understanding Subscriptions and Software Repositories

OpenShift is built on top of Red Hat Enterprise Linux. The installation procedure adds additional software repositories and leverages an ansible inventory file as a centralized place to configure and automate the installation of OpenShift across multiple machines.

Because Red Hat software is made available via subscriptions, a common production task is to target subscriptions to exact machines. A common workflow is to register a host, select the correct subscription and repositories, and install or update update software as needed. Solutions like Red Hat Satellite can automate the registration and maintenance of Red Hat software. 

We have pre-arranged the subscriptions used in this class, so we will not be registering our machines. We are using yum-config-manager today however production environments would leverage subscription-manager to locally administer subscriptions and software repositories. yum-config-manager will approximate the process of adding the correct subscriptions to an OpenShift deployment.

## Lab Instructions
Our first step is to ensure we have the proper software repositories enabled. We start by disabling any existing repositories and adding only the repositories we need. 

1. On your workstation machine, execute the following to become a root user:
```
[student@workstation ~]$ sudo -i '
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
7. Save and exit the file.
```
[root@workstation OCP-Workshop]#
```

4. Prepare the cluster by installing all the required packages for OpenShift:
```
[root@workstation ~]$ time ansible-playbook -i configs/configs/ocp-3.11-workshop ./playbooks/ocp-3.11-prep-workstation.yml
```

5. Confirm the correct repositories have been enabled with the following:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'yum repolist'
```

You should see output similar to the following:
```
...
master | SUCCESS | rc=0 >>
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-
              : manager
This system is not registered with an entitlement server. You can use subscription-manager to register.
repo id                                 repo name                         status
rhel-7-fast-datapath-rpms               RHEL7 FAST DATAPATH                  29
rhel-7-server-ansible-2.4-rpms          RHEL7 Server Ansible 2.4             10
rhel-7-server-extras-rpms               RHEL7 EXTRAS                        105
rhel-7-server-ose-3.10-rpms             RHEL7 OSE 3.10                      524
rhel-7-server-rpms                      RHEL7                             5,285
repolist: 5,953
...
```

In a production environment, it is important to understand how to manage subscriptions, both to receive support and to ensure software remains current and secure.

[Lesson Three: Install the Initial Software Packages](03-lesson-install_initial_software.md)
