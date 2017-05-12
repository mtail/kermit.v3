Scality S3 Server
=================

![S3 Server logo](res/Scality-S3-Server-Logo-Large.png)


S3 Server on Kermit
===========================

This is a quickstart S3 Server application for OpenShift v3 that you can use as a starting point to develop your own application and deploy it on Kermit.

If you'd like to install it using the Web Console, follow [these directions](http://kermit-docker.rd.francetelecom.fr/knowledge-base/deploy-s3-scality-server/).  

This example was uses the code from the official [Scality S3 Server](https://github.com/scality/S3) repository.

The steps in this document assume that you have access to an OpenShift deployment that you can deploy applications on.

Installation
------------

1. Fork a copy of [s3-server](https://qzdh0054@gitlab.forge.orange-labs.fr/kermit/Quickstarts/s3-server.git)

2. Clone your repository to your development machine and cd to the repository directory

3. Add a S3 Server application from the provided template and specify the source url to be your forked repo  

		$ oc new-app openshift/templates/s3-server.yaml -p SOURCE_REPOSITORY_URL=<your repository location>

4. Depending on the state of your system, and whether additional items need to be downloaded, it may take around a minute for your build to be started automatically.  
   If you do not want to wait, run

		$ oc start-build s3-server

5. Once the build is running, watch your build progress  

		$ oc logs build/s3-server-1

6. Wait for S3 Server pods to start up (this can take a few minutes):  

		$ oc get pods -w

	Sample output:  

        NAME                   READY     STATUS              RESTARTS   AGE
        s3-server-1-build      0/1       Completed           0          1m
        s3-server-1-deploy     0/1       ContainerCreating   0          5s

7. Check the IP and port the wordpress service is running on:  

		$ oc get svc

	Sample output:  

        NAME               CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        s3-server-svc      172.30.170.134   <none>        8080/TCP   3m

*Note*: you can also get this information from the web console.

### Installation: With Persistent Storage

1. Follow the steps for the Manual Installation above for all but step 3, instead use step 2 below.  
  - Note: The output in steps 5-6 may also display information about your database.

2. Add a Wordpress application from the s3-server-persistent template and specify the source url to be your forked repo  

		$ oc new-app openshift/templates/s3-server-persistent.yaml -p SOURCE_REPOSITORY_URL=<your repository location>

Testing
-------

### Accessing the S3 Server 

   Default:
    
        access_key=accessKey1
        secret_key=verySecretKey1

### Setting your own access key and secret key pairs

You can set credentials by:

  - Set the environment variables `SCALITY_ACCESS_KEY_ID` and `SCALITY_SECRET_ACCESS_KEY`
  - Change the configuration file `conf/authdata.json` 

### Configuration

There are three configuration files for your Scality S3 Server:

1. `conf/authdata.json`, described above for authentication

2. `locationConfig.json`, to set up configuration options for where data will be saved

3. `config.json`, for general configuration options

### Endpoints

Note that our S3server supports both:

- path-style: http://myhostname.com/mybucket
- hosted-style: http://mybucket.myhostname.com

However, hosted-style requests will not hit the server if you are
using an ip address for your host.
So, make sure you are using path-style requests in that case.


Getting started
---------------

### GUI



#### [S3 Browser](http://s3browser.com/)

#### [CloudBerry Explorer](https://www.cloudberrylab.com/explorer/amazon-s3.aspx)

  - Account name : S3 Server
  - Storage type : S3 Compatible storage
  - REST Endpoint : S3 Server URL ([APPLICATION_NAME]-[PROJECT_NAME].kermit.itn.intraorange)
  - Access key ID : accessKey1
  - Secret access key : verySecretKey1

### Command Line Tools

#### [s3curl](https://github.com/rtdp/s3curl)

https://github.com/scality/S3/blob/master/tests/functional/s3curl/s3curl.pl

#### [s3cmd](http://s3tools.org/s3cmd)

If using s3cmd as a client to S3 be aware that v4 signature format
is buggy in s3cmd versions < 1.6.1.

`~/.s3cfg` on Linux, OS X, or Unix or
`C:\Users\USERNAME\.s3cfg` on Windows

```shell
[default]
access_key = accessKey1
secret_key = verySecretKey1
host_base = s3_server_hostname ### [APPLICATION_NAME]-[PROJECT_NAME].kermit.itn.intraorange
host_bucket = s3_bucket_name   ### s3://bucket-name
signature_v2 = False
use_https = False
```

See all buckets:

```shell
s3cmd ls
```