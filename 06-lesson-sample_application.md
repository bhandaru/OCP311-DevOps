# Lesson Six: Creating a Sample Application via the Web UI

We created a user in the previous lesson, cleverly named `user`. Now we will deploy a sample application that leverages an existing container image and also builds another container via source code. For this example, we’re adapting a lesson from https://learn.openshift.com. 

While we can create applications from the command line, we will demonstrate how to use the web interface. Because of the environment we’re using, you will need the public IP address of the master host used when creating the inventory file. The actual URL will look something like `129.146.147.7.xip.io`.

## Lab Instructions

1. Using a web browser, use the following syntax to log into your master host: 
```
https://YOUR_MASTER_IP_ADDRESS.xip.io:8443
```
Log in with the `user` account you created earlier and perform the following steps:

2. Click the blue *Create Project* button and name the project `myproject`. The other fields are optional. After creating the project, click the blue *myproject* link.

3. Next click *Deploy Image*. This dialog allows us to deploy existing containers. Select the *Image Name* radio button and input: `openshiftroadshow/parksmap-katacoda:1.0.0`. Press return, then the blue *Deploy* button on the bottom right. 

4. Next click *Continue to Project Overview*. You’ll see an overview of *myproject* and can interact with the number of containers running, among many other controls. From this view, you can increase the number of running containers, view logs in real time, and take many other actions.

5. In order to interact with your application outside of the OpenShift cluster itself, you need to create a route. In the overview, ensure the arrow to the left of *parksmap-katacoda* is expanded. 

6. Click *Create Route* and accept the defaults by clicking *Create* at the bottom of the page. You’ll see a URL that you can click within the overview.

7. Click the route URL, you should see a map in a browser window.

8. We will now create a container from existing source code, also called “S2I” or “source to image”. This container will be the backend for our map.

9. Switch back to the Console tab and click *Add to Project* in the upper right corner, then *Browse Catalog*. Find and select *Python*. Ensure the *Application Name* is: `nationalparks-katacoda` and the *Git Repository* is:
```
https://github.com/openshift-roadshow/nationalparks-katacoda
```

10. Click the blue *Create* button in bottom corner.

11. Then click on *Continue to the Project Overview* where you can watch the build progress.

Once the build completes, you can click on the first route you created earlier to see your map populated.

[Extra Credit Lesson: Using the CLI to Create Applications](07-lesson-extra_credit.md)
