# Lesson One: Getting Familiar with the Environment

These steps are to be performed after your instructor has provisioned an environment for you. Using an ssh client from your laptop, you will log into the workstation machine. You will log in using the “student” user name, and your instructor will provide the password. 

Your workstation virtual machine has a unique, publicly-accessible IP address and DNS name. Your OpenShift machines will communicate via private IP addresses. Because these addresses cannot be routed to your laptop, the workstation acts as a jump host and can access the entire hosted environment.

Once you've logged into the portal using the student credentials provided by your instructor, proceed to the guestbook and register. This registration process will give you a student number and will provide the public IP address of the workstation. In a later lab, this page will also provide a graphical environment.

## User laptop software requirements

#### Microsoft Windows
1. SSH Client: Putty for Windows
   Download ssh client for Windows at https://putty.org if need be.

2. If ssh is blocked on the network, then use https://shellngn.com to do ssh over http. You will need to setup an account if you do not have one already.

#### Linux
1. Use ssh from a terminal window.
2. If ssh client package is not installed then
    - For Linux: Run `yum -y install openssh-clients`

## Lab Instructions:

1. Open a command line window or a terminal on your laptop.

2. ssh to the workstation host in the OpenShift cluster. Get the IP Address to your workstation in the OpenShift Cluster from the URL on the whiteboard. Use your assigned student# to find the IP address.
```
ssh student@<workstation IP>
```

3. Build an ansible hosts file suitable for managing your OpenShift environment:
```
[student@workstation ~]$ sudo vim /etc/ansible/hosts
[ocp]
master
node1
node2
node3
docker
```

This file allows you to manage your OpenShift environment with ansible. Ansible allows us to easily execute time-consuming commands in parallel during our workshop, among other advantages.

Ansible requires that hosts be reachable without intervention. For this training, we've created keys that allow password-less logins via root. See the appendix for additional information about creating and distributing ssh keys.

4. Try the following commands to ensure your OpenShift virtual machines are reachable via ansible:
```
[student@workstation ~]$ sudo ansible all -m ping
```
If everything is configured properly, you should see something like the following:
```
node1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
node2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
master | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
5. You may also execute arbitrary shell commands via ansible:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'uptime'
```
The output should be similar to the sample output below.
```
node2 | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.07, 0.08, 0.08

node1 | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.27, 0.15, 0.15

master | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.55, 0.62, 0.77
```
Review your results to ensure it matches the sample output above, confirming that ansible is working properly and your OpenShift virtual machines are reachable before proceeding.

[Lesson Two: Understanding Subscriptions and Software Repositories](02-lesson-understanding_subscriptions.md)
