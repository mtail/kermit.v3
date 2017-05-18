
Workshop Tutorial on Kermit
===========================

This is a totorial for the workshop application for OpenShift v3 that you can use as a starting point to develop your own application and deploy it on Kermit.

If you'd like to install it using the Web Console, follow [these directions](http://kermit-docker.rd.francetelecom.fr/knowledge-base/1-hour-tutorial-using-web-console/).

The steps in this document assume that you have access to an OpenShift deployment that you can deploy applications on.

Installation
------------

1. Fork a copy of [Workshop](https://gitlab.forge.orange-labs.fr/kermit/tutorials/workshop.git)

2. Clone your repository to your development machine and cd to the repository directory

3. Add a Workshop application from the provided template and specify the source url to be your forked repo  

		$ oc new-app openshift/templates/workshop.yaml -p SOURCE_REPOSITORY_URL=<your repository location>

4. Depending on the state of your system, and whether additional items need to be downloaded, it may take around a minute for your build to be started automatically.  
   If you do not want to wait, run

		$ oc start-build workshop

5. Once the build is running, watch your build progress  

		$ oc logs build/workshop-1

6. Wait for Workshop pods to start up (this can take a few minutes):  

		$ oc get pods -w

	Sample output:  

        NAME                   READY     STATUS              RESTARTS   AGE
        workshop-1-build      0/1       Completed           0          1m
        workshop-1-deploy     0/1       ContainerCreating   0          5s

7. Check the IP and port the Workshop service is running on:  

		$ oc get svc

	Sample output:  

        NAME               CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        workshop-svc      172.30.170.134   <none>        8080/TCP   3m

*Note*: you can also get this information from the web console.

### Installation: With Persistent Storage

1. Follow the steps for the Manual Installation above for all but step 3, instead use step 2 below.  
  - Note: The output in steps 5-6 may also display information about your database.

2. Add a Workshop application from the s3-server-persistent template and specify the source url to be your forked repo  

		$ oc new-app openshift/templates/workshop-mysql-persistent.yaml -p SOURCE_REPOSITORY_URL=<your repository location>
