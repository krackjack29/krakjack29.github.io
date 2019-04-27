---
title: "Restricting Azure APP service accessible only via an Application Gateway"
layout: single
comments: true
date: 2019/04/07
categories:
    - Azure
tags:
    - azure
    - cloud
    - appservice
    - appgateway
---

Azure app service provides a publicly accessible endpoint for the application you deploy in it. One can restrict the access by configuring an *Isolated* tier app service plan and restricting the application to the Virtual network only. But, *Isolated* app service plan (I1 costs about $301) is at least 4 times costlier than a standard (S1 costs about $73) instance and should be used if you have more than 1 applications to host internally within the network. 

In the scenario where an already deployed web application using an App service public plan has to be integrated with application gateway and your clients should access the website only via the application gateway and not the publicly exposed endpoints of app service, you can do so using the IP restriction feature of the application service.

1. Find out the public ip address of the application gateway, you can do so by looking up the front-end ip configuration of the application gateway ![front end ip config](/assets/images/restricip/frontendip.png)

2. Navigate to the IP address and copy the address to clipboard, we will be using it later ![ip address](/assets/images/restricip/ip.png)

3. Open the deployed app service from azure portal, select "Networking" and click on Access restrictions ![Access restrictions](/assets/images/restricip/access.png)

4. Click on Add rule, and enter the frontend ip address of the application gateway. The default "allow all" would change to "deny all" once you add an ip. ![Rule](/assets/images/restricip/last.png)
   

After following above steps and trying to access the https://{appName}.azurwebsites.net would provide a 403 error with message "This web app is stopped" but if you try from the front end ip or a domain name configured in the application gateway it would display the expected website. 

**Things to consider**
* This feature is an enhancement on the IP white-listing feature of IIS hence would work only with applications deployed on Windows APP service plans. 
* This will not stop DDoS attacks as the requests would still be reaching your application, which means any attacker could reach the destination and keep the server busy. 

Happy Coding!!!!

**References**
[https://docs.microsoft.com/en-us/azure/app-service/app-service-ip-restrictions](https://docs.microsoft.com/en-us/azure/app-service/app-service-ip-restrictions)