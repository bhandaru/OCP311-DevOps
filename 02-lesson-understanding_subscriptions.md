# Lesson Two: Understanding Subscriptions and Software Repositories

OpenShift is built on top of Red Hat Enterprise Linux. The installation procedure adds additional software repositories and leverages an ansible inventory file as a centralized place to configure and automate the installation of OpenShift across multiple machines.

Because Red Hat software is made available via subscriptions, a common production task is to target subscriptions to exact machines. A common workflow is to register a host, select the correct subscription and repositories, and install or update update software as needed. Solutions like Red Hat Satellite can automate the registration and maintenance of Red Hat software. 

We have pre-arranged the subscriptions used in this class, so we will not be registering our machines. We are using yum-config-manager today however production environments would leverage subscription-manager to locally administer subscriptions and software repositories. yum-config-manager will approximate the process of adding the correct subscriptions to an OpenShift deployment.

## Lab Instructions
Our first step is to ensure we have the proper software repositories enabled. We start by disabling any existing repositories and adding only the repositories we need. 

1. On your workstation machine, execute the following:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'yum-config-manager  --disable \* '
```

2. Enable the needed repos for all OpenShift hosts:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'yum-config-manager --enable \ 
    rhel-7-server-rpms \
    rhel-7-server-extras-rpms \
    rhel-7-server-ose-3.10-rpms \
    rhel-7-fast-datapath-rpms rhel-7-server-ansible-2.4-rpms'
```

3. Confirm the correct repositories have been enabled with the following:
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
