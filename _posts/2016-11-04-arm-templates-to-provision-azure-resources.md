title: ARM Templates to provision Azure Resources
link: https://pratapgowda.wordpress.com/2016/11/04/arm-templates-to-provision-azure-resources/
author: pratapgowda
description: 
post_id: 246
created: 2016/11/04 13:02:00
created_gmt: 2016/11/04 07:32:00
comment_status: open
post_name: arm-templates-to-provision-azure-resources
status: publish
post_type: post

# ARM Templates to provision Azure Resources

<p>Azure Resource Manager (ARM) templates are quite handy when it comes to automating the provisioning of azure resources. Almost 99% of the azure resources can now be provisioned using these templates.</p> <p>Microsoft also has quick start templates for all the resources in <a href="https://github.com/Azure/azure-quickstart-templates" target="_blank">GitHub</a>. If you notice the quick start templates they normally have two files in each example </p> <ol> <li>azuredeploy.json - contains information of resources that you are going to provision, the json object contains 4 objects internally.</li> <ol> <li>parameters : this section allows you to define the inputs for the deployments such as name of the resources etc.</li> <li>variables : contains calculated variables to be used within the scope of the deployment json.</li> <li>resources : resources that are to be deployed, you can set the resource order of deployment using “dependsOn” property.</li> <li>outputs: define the output after the deployment is successful, could be a connection string to a SQL database you deployed, SAS key to the eventhub etc.</li></ol> <li>azuredeploy.parameters.json – contains the parameter values defined in azuredeploy.json.</li></ol> <p>You can deploy the ARM templates using PowerShell, or command line (Azure CLI commands need to be installed for this). To execute above options you would need the azuredeploy.json file to have a open uri endpoint and provide that endpoint as the input to the commands.</p> <p>You can also deploy the templates using REST APIs and a .Net client. Here is the sample application in <a href="https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-csharp-template/" target="_blank">MSDN</a>. </p> <p>-- Happy Coding</p>