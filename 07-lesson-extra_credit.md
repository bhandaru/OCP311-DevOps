# Extra Credit Lesson: Using the CLI to Create Applications

While the previous steps demonstrate how to use the web console, we will now show how the same application can be installed on the command line. We will perform these steps on master.

## Lab Instructions

1. Become `user` on master, and verify the environment:
```
[root@master ~]# oc login -u user
Logged into "https://master.example.com:8443" as "user" using existing credentials.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```

```
[root@master ~]# oc whoami
user
```
```
[root@master ~]# oc whoami --show-server
https://master.example.com:8443
```
```
[root@master ~]# oc projects
You have one project on this server: "myproject".

Using project "myproject" on server "https://master.example.com:8443".
```
2. Recreate *myproject*. To do this, we will first delete the project created in the previous step:
```
[root@master ~]# oc delete project myproject
project.project.openshift.io "myproject" deleted
```

Create project `my-project`
```
[root@master ~]# oc new-project myproject
Already on project "myproject" on server "https://master.example.com:8443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git

to build a new example application in Ruby.
```

3. Next we will import the first container our project depends on:
```
[root@master ~]# oc new-app bhandaru/parksmap-katacoda:1.0.2
--> Found Docker image 7722b79 (17 months old) from Docker Hub for "openshiftroadshow/parksmap-katacoda:1.0.0"

    Python 3.5
    ----------
    Platform for building and running Python 3.5 applications

    Tags: builder, python, python35, rh-python35

    * An image stream will be created as "parksmap-katacoda:1.0.0" that will track this image
    * This image will be deployed in deployment config "parksmap-katacoda"
    * Port 8080/tcp will be load balanced by service "parksmap-katacoda"
      * Other containers can access this service through the hostname "parksmap-katacoda"

--> Creating resources ...
    imagestream "parksmap-katacoda" created
    deploymentconfig "parksmap-katacoda" created
    service "parksmap-katacoda" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/parksmap-katacoda'
    Run 'oc status' to view your app.
```
```
[root@master ~]# oc status
In project myproject on server https://master.example.com:8443

svc/parksmap-katacoda - 172.30.238.97:8080
  dc/parksmap-katacoda deploys istag/parksmap-katacoda:1.0.0
    deployment #1 deployed 47 seconds ago - 1 pod


2 infos identified, use 'oc status -v' to see details.

[root@master ~]# oc get all
NAME                            READY     STATUS    RESTARTS   AGE
pod/parksmap-katacoda-1-9llrx   1/1       Running   0          46s

NAME                                        DESIRED   CURRENT   READY     AGE
replicationcontroller/parksmap-katacoda-1   1         1         1         51s

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/parksmap-katacoda   ClusterIP   172.30.238.97   <none>        8080/TCP   52s

NAME                                                   REVISION   DESIRED   CURRENT   TRIGGERED BY
deploymentconfig.apps.openshift.io/parksmap-katacoda   1          1         1         config,image(parksmap-katacoda:1.0.0)

NAME                                               DOCKER REPO                                                    TAGS      UPDATED
imagestream.image.openshift.io/parksmap-katacoda   docker-registry.default.svc:5000/myproject/parksmap-katacoda   1.0.0     51 seconds ago
```

4. As suggested once we imported the existing container, we need to add a route:
```
[root@master ~]# oc expose svc/parksmap-katacoda
route "parksmap-katacoda" exposed

[root@master ~]# oc get routes
NAME                HOST/PORT                                       PATH      SERVICES            PORT       TERMINATION   WILDCARD
parksmap-katacoda   parksmap-katacoda-myproject.cloud.example.com             parksmap-katacoda   8080-tcp                 None
```

5. Note the URL shown via `oc get routes`. That’s the URL our map will be available on. We can list the containers of our application by running the following:
```
[root@master ~]# oc get pods
NAME                        READY     STATUS    RESTARTS   AGE
parksmap-katacoda-1-9llrx   1/1       Running   0          7m
```

6. Next we need the backend to our application. We will create a container from existing source code. Run the following:
```
[root@master ~]# oc new-app https://github.com/openshift-roadshow/nationalparks-katacoda --name=nationalparks-katacoda
--> Found image 3eb1852 (3 weeks old) in image stream "openshift/python" under tag "3.6" for "python"

    Python 3.6
    ----------
    Python 3.6 available as container is a base platform for building and running various Python 3.6 applications and frameworks. Python is an easy to learn, powerful programming language. It has efficient high-level data structures and a simple but effective approach to object-oriented programming. Python's elegant syntax and dynamic typing, together with its interpreted nature, make it an ideal language for scripting and rapid application development in many areas on most platforms.

    Tags: builder, python, python36, rh-python36

    * The source repository appears to match: python
    * A source build using source code from https://github.com/openshift-roadshow/nationalparks-katacoda will be created
      * The resulting image will be pushed to image stream "nationalparks-katacoda:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "nationalparks-katacoda"
    * Port 8080/tcp will be load balanced by service "nationalparks-katacoda"
      * Other containers can access this service through the hostname "nationalparks-katacoda"

--> Creating resources ...
    imagestream "nationalparks-katacoda" created
    buildconfig "nationalparks-katacoda" created
    deploymentconfig "nationalparks-katacoda" created
    service "nationalparks-katacoda" created
--> Success
    Build scheduled, use 'oc logs -f bc/nationalparks-katacoda' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/nationalparks-katacoda'
    Run 'oc status' to view your app.
```

7. We can check the number of containers again:
```
[root@master ~]# oc get pods
NAME                             READY     STATUS      RESTARTS   AGE
nationalparks-katacoda-1-build   0/1       Completed   0          1m
nationalparks-katacoda-1-wlt4z   1/1       Running     0          22s
parksmap-katacoda-1-9llrx        1/1       Running     0          13m
```

8. After the build is finished, you can input the route created earlier into your browser to view your map.
```
[root@master ~]# oc get routes
NAME                HOST/PORT                                       PATH      SERVICES            PORT       TERMINATION   WILDCARD
parksmap-katacoda   parksmap-katacoda-myproject.cloud.example.com             parksmap-katacoda   8080-tcp                 None
```
[Final Thoughts](08-lesson-final_thoughts.md)
