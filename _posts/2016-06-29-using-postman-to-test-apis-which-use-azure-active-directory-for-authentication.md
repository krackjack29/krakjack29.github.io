---
title: Using postman to test Apis which use Azure Active Directory for Authentication
date: 2016/06/29 00:30:00 +530
layout: single
comments: true
categories: 
   - Tech
tags:
   - tools
   - api
   - postman
   - azure
   - cloud
---

Postman is arguably the best tool for developers to test/debug their APIs. If you haven’t heard about it or used it I recommend to go download it [ASAP](http://www.getpostman.com/).

Postman has lot of authentication helpers to authorize your API calls, you can read about all of them [here](http://www.getpostman.com/docs/helpers). Our application was using Azure Active Directory to authorize users, the bearer token was being set by the UI, to test the APIs we were using Postman to set the same.

Easier way is to copy the generated token into the authorization header, but its better and cleaner to do it through postman. 

Prerequisite would be to create an Azure AD application in the Azure portal, which delegates permission to your actual application. If you know how to create a Azure application skip to the next part.

1. Navigate to https://manage.windowsazure.com

2. Select the Azure Directory instance where you resource application is created.

3. Click Add application, you would see the wizard as below

![image](/assets/images/poststep3.png)

4. Select Add an Application my organization is developing.

5. In the next screen type in some name e,g, postman and Type as “Web Application AND/OR Web API”, click next

![screen2](/assets/images/poststep5.png)

6. “Sign-On URL” and “App ID URL” can be any valid URL

![screen2](/assets/images/poststep6.png)

7. Once created, click the application you just created (lets call it “postman-test”) and navigate to Configure tab.

   * Copy the Client Id into a notepad
   * Create a Key with 1 year validity and copy it to some notepad, will be used later as Client Secret 
   * At bottom you would find “permissions to other applications” click on “Add Application”, select the actual application you want to authorize, in my case its IotServiceDev 
![screen2](/assets/images/poststep7.png)
   * Change the reply Url to “https://www.getpostman.com/oauth2/callback” , this is very important.

Now to test the APIs of IotServiceDev (or your actual under test) application launch postman app. In the Authorization tab, select OAuth2.0 and click on “Get New Access Token”

![screen2](/assets/images/poststep8.png)

You would find a form such as the above, fill the form with following details

* Token Name – Any name to save the token.
* Auth Url – “https://login.microsoftonline.com/{tenant}/oauth2/authorize?resource={testing-appId-uri}”
* Access Token Url – “https://login.microsoftonline.com/{tenant}/oauth2/token”
* Client ID – Client Id from configure tab of “postman-test” app.
* Client Secret – Client secret copied from configure tab of “postman-test” app.

Note: tenant– It can be either the name of the active directory or TenantId of the admin who created the active directory. testing-appId-uri is the App ID Uri of the application you are testing.

Click on request token ,it would take you Microsoft azure login page. Once you provide the right credentials, token would be listed with the name you specified in the form. For the APIs you want to test click on the token, and you will get option of either sending via header (as bearer token) or append to URL as query parameter.

Happy Coding!

