title: Continuous Deployment of Service Fabric Application
link: https://pratapgowda.wordpress.com/2016/08/11/continuous-deployment-of-service-fabric-application/
author: pratapgowda
description: 
post_id: 241
created: 2016/08/11 21:21:00
created_gmt: 2016/08/11 15:51:00
comment_status: open
post_name: continuous-deployment-of-service-fabric-application
status: publish
post_type: post

# Continuous Deployment of Service Fabric Application

<p>Once an application is deployed to a service fabric cluster, you have two options to deploy your existing code. </p> <p>1. Redeploy – this would remove the existing application and wipe out the State of the services in the application. you wouldn't want that now, do you?.</p> <p>2. Upgrade the existing services – This is most apt of pushing your latest code and still maintain the state of the actors and services. Esp. when you have a continuous deployment in place.</p> <p>The configurations for each services is stored in “ServiceManifest.xml” of each service, placed under the folder of PackageRoot.</p> <p><a href="http://pratapgowda.files.wordpress.com/2016/08/solution.png"><img title="solution" style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border-width:0;" border="0" alt="solution" src="http://pratapgowda.files.wordpress.com/2016/08/solution_thumb.png" width="195" height="270"></a></p> <p>A sample service manifest would be as shown below</p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:9f8fc3f9-8545-45e7-b7ab-808b832f6f70" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="white-space:normal;">