# Lesson Five: Interacting With Your Installation

The prior lessons build on each other. If there’s an issue along the way, it’s very unlikely the OpenShift cluster will be installed. All errors should be investigated, and many can be addressed in the inventory file.

## Lab Instructions

1. Log onto master, and run the following to get information about the cluster.
```
[root@workstation OCP-Workshop]# ssh master

[root@master ~]# oc get nodes
NAME                 STATUS    ROLES          AGE       VERSION
master.example.com   Ready     infra,master   1h        v1.10.0+b81c8f8
node1.example.com    Ready     infra          1h        v1.10.0+b81c8f8
node2.example.com    Ready     compute        1h        v1.10.0+b81c8f8
node3.example.com    Ready     compute        1h        v1.10.0+b81c8f8
```

2. Run the following to get additional information about the cluster.
```
[root@master ~]# oc get nodes -o wide
NAME                 STATUS    ROLES          AGE       VERSION           EXTERNAL-IP   OS-IMAGE                   KERNEL-VERSION              CONTAINER-RUNTIME
master.example.com   Ready     infra,master   1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
node1.example.com    Ready     infra          1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
node2.example.com    Ready     compute        1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
node3.example.com    Ready     compute        1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
```

3. Run the following to get information about the projects deployed.
```
[root@master ~]# oc get projects
NAME                                DISPLAY NAME   STATUS
default                                            Active
kube-public                                        Active
kube-service-catalog                               Active
kube-system                                        Active
management-infra                                   Active
openshift                                          Active
openshift-ansible-service-broker                   Active
openshift-console                                  Active
openshift-infra                                    Active
openshift-logging                                  Active
openshift-metrics-server                           Active
openshift-monitoring                               Active
openshift-node                                     Active
openshift-node-problem-detector                    Active
openshift-sdn                                      Active
openshift-storage                                  Active
openshift-template-service-broker                  Active
openshift-web-console                              Active
operator-lifecycle-manager                         Active

```

Your output should look similar to the above. 

4. After exploring a bit, attempt a login from the command line on master:
```
[root@master ~]# oc login https://master.example.com:8443
```

Wondering what username and password to use? No user credentials are specified yet.
```
[root@master ~]# oc login https://master.example.com:8443
Authentication required for https://master.example.com:8443 (openshift)
Username: admin
Password:
Login failed (401 Unauthorized)
Verify you have provided correct credentials.
```

Referring to our OpenShift inventory file, we had specified `htpasswd` as a valid authentication method in our inventory file:
```javascript
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]
```

5. Create an admin user and password that will allow login and see additional information:
```
[root@master ~]# htpasswd -b /etc/origin/master/htpasswd admin ocpadmin
Updating password for user admin
You have new mail in /var/spool/mail/root
```
A user named `admin` with the password of `ocpadmin` is now created.

6. Give `cluster-admin` role to user `admin`.
```
[root@master ~]# oc adm policy add-cluster-role-to-user cluster-admin admin
cluster role "cluster-admin" added: "admin"
```

7. Log in again using User ID `admin`:
```
[root@master ~]# oc login https://master.example.com:8443
Authentication required for https://master.example.com:8443 (openshift)
Username: admin
Password:
Login successful.

You have access to the following projects and can switch between them with 'oc project <projectname>':

  * default
    kube-public
    kube-service-catalog
    kube-system
    management-infra
    openshift
    openshift-ansible-service-broker
    openshift-infra
    openshift-logging
    openshift-node
    openshift-sdn
    openshift-template-service-broker
    openshift-web-console

Using project "default".
```

8. Try a few different commands once logged in:
```
[root@master ~]# oc whoami
admin
```
```
[root@master ~]# oc whoami --show-server
https://master.example.com:8443
```
```
[root@master ~]# oc get projects
NAME                                DISPLAY NAME   STATUS
default                                            Active
kube-public                                        Active
kube-service-catalog                               Active
kube-system                                        Active
management-infra                                   Active
openshift                                          Active
openshift-ansible-service-broker                   Active
openshift-console                                  Active
openshift-infra                                    Active
openshift-logging                                  Active
openshift-metrics-server                           Active
openshift-monitoring                               Active
openshift-node                                     Active
openshift-node-problem-detector                    Active
openshift-sdn                                      Active
openshift-storage                                  Active
openshift-template-service-broker                  Active
openshift-web-console                              Active
operator-lifecycle-manager                         Active

```

9. Next, create two user accounts with limited authority. Perform the following on master:
```
[root@master ~]# htpasswd -b /etc/origin/master/htpasswd user ocpuser
Adding password for user user
[root@master ~]# htpasswd -b /etc/origin/master/htpasswd student <provided password>
Adding password for user student
```

10. Now, there will be a user named `user` with the password of `ocpuser`, as well as a user named `student` with the password of `<provided password>`. Log in via the CLI by doing the following:
```
[root@master ~]# oc whoami
admin
```

```
[root@master ~]# oc login -u user
Authentication required for https://master.example.com:8443 (openshift)
Username: user
Password:
Login successful.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```

```
[root@master ~]# oc whoami
user

[root@master ~]# oc whoami --show-server
https://master.example.com:8443
```

You can change users on the command line using `oc login`. It’s a good idea to determine which user is active with `oc whoami` before interacting with OpenShift. 

For the remaining labs, we will be using the `user` OpenShift account.

[Lesson Six: Creating a Sample Application via the Web UI](06-lesson-sample_application.md)
