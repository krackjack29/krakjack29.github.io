title: Deploying ARM Template and parsing the outputs in a .Net Application
link: https://pratapgowda.wordpress.com/2016/11/04/deploying-arm-template-and-parsing-the-outputs-in-a-net-application/
author: pratapgowda
description: 
post_id: 250
created: 2016/11/04 15:54:00
created_gmt: 2016/11/04 10:24:00
comment_status: open
post_name: deploying-arm-template-and-parsing-the-outputs-in-a-net-application
status: publish
post_type: post

# Deploying ARM Template and parsing the outputs in a .Net Application

<p>There are scenarios where one might have to provision a Azure resource on the fly and use the connection string or SAS key or credentials of those deployed resources further in your application. </p> <p>In this blog post we will deploy an Azure IotHub and get the SAS key to do further device management operations.</p> <p>1. A simple ARM template to deploy the Azure IotHub would look like <a href="https://github.com/pratapbhaskar/arm-template-deployer/blob/master/ArmTemplateDeployer/iotHubDeploy.json" target="_blank">this</a>. It contains 4 parameters namely iotHubName, iotHubSku, iotHubTier, iotHubCapacity all the parameters have a default value set except for iotHubName . It emits two ouputs</p> <p><a href="http://pratapgowda.files.wordpress.com/2016/11/image.png"><img title="image" style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border-width:0;" border="0" alt="image" src="http://pratapgowda.files.wordpress.com/2016/11/image_thumb.png" width="503" height="127"></a></p> <p>iotHubHostName , is the name of the resource that we just deployed, while iotHubKeys would emit all the SAS keys generated. </p> <p>2. You need service principal and secret to get the access token. ResourceManagerProvider class has a method called GetResourceManagementClient which gets the token using ServicePrincipalId and ServicePrincipalSecret.</p> <p>&nbsp;</p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:acf68252-52ef-4287-bc47-71d06b1673d1" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="white-space:normal;">