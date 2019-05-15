# Lesson Three: Install the Initial Software Packages

With the proper software repositories enabled, you can now install the initial packages needed for OpenShift. OpenShift's installation is a multi-step process. We describe the installation of the initial packages, and later we will discuss the creation of the OpenShift inventory file and subsequent steps.

## Lab Instructions

1. Run the following from your workstation to install the initial packages:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'yum install -y \
  wget git net-tools \
  bind-utils yum-utils \
  iptables-services bridge-utils \
  bash-completion kexec-tools \
  sos psacct openshift-ansible \
  docker vim screen'
```
The initial package installation shouldn’t take too long to complete.

2. OpenShift requires that NetworkManager be enabled on the cluster. Enable NetworkManager by running the following:
```
[student@workstation ~]$ sudo ansible all -m shell -a 'systemctl enable NetworkManager'
```

At this point, you should have the initial packages needed to begin the OpenShift installation in place.

[Lesson Four: Container Image Storage](04-lesson-container_image_storage.md)
