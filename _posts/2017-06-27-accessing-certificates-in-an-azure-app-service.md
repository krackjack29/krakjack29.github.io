---
title: "Accessing Certificates in an Azure App Service"
date: 2017/06/27 13:56:08 +530
layout: single
comments: true
categories: 
    - Azure
tags:
    - azure
    - cloud
    - appservice
---

There are number of reasons one would want to load a certificate (*.pfx, *.p12) with **private key**, mine was to sign a JWT token. Requirement was to sign JWT with private key and validate it on the client application using public key. This signing application was to be hosted in Azure App Service.

When we try to load a certificate using .Net’s x509Certificate2 class, when private key is included application needs to be running in elevated privilege or atleast “Load user profile” needs to be set if its running on IIS. Now neither is possible when application is running in Azure App Service.

### How do we access it?
1. Login to new azure portal. https://portal.azure.com
2. Select your Azure website from the Websites blade.
3. Select “SSL Certificates” option from the app blade azure portal ![step3](/assets/images/certstep3.png)
4. Click on “Upload Certificate”.
5. Select the certificate (Pfx only) and enter the password for the certificate. If its a valid certificate, you would get a success message.

![step5](/assets/images/certstep5.png)

Once certificate is uploaded successfully, you would see it in the list as shown in the first picture.

### Accessing Certificate from application

There is one more configuration that needs to be done, before we dwell into the code. In the appsettings section add **“WEBSITE_LOAD_CERTIFICATES”** as key and a comma separated thumbprint values as shown in the SSL certificates list.

![appsettings](/assets/images/certsettings.png)

In your application code, read from the store as below.

```csharp
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var certCollection = store.Certificates.Find(X509FindType.FindByThumbprint,
                    <<Insert_Thumbprint_here>>, false);
if (certCollection.Count > 0)
{
        certificate = certCollection[0];
}
```

Happy Coding!!!

