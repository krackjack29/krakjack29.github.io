---
title: "Performance Tuning for ASP.Net applications"
date: 2014/03/05 15:47:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - csharp
   - web
   - asp.net
---

What I have noticed in most of the projects that I have been part of is that the Performance is considered as a least priority feature. Well its definitely not a feature, performance should be built in each feature and measured as early and frequently as possible.

I have collated few key points to fine tune any web application’s (targeting .Net and IIS but not limited to) performance.

### Use CDN (Content Delivery network)
All the 3rd party JavaScript files such as JQuery, Knockout should always use the CDN instead of the web application server. CDN Servers are dedicated to deliver the static content and is always faster than your own host.

There is a very high probability that the client (browser) would have already cached the JavaScript as part of other web application since most of them use the same CDN url. You can read more about the benefits about CDN here.

### Use Bundling and Minification
The custom CSS files and the JavaScript files should be bundled into a single large file (reduces the number of HTTP requests) and also minified (reduces the size of the data transferred over the wire).

How to enable bundling and minification in MVC: http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification

### Use static content caching
Always set the static content to be cached (includes JavaScript, CSS, images etc.) on the client side. Most modern day browsers would cache the static content themselves. Use “Never Expires” policy to ensure that most of them needn’t be updated.

**Note**: This could also lead to the client not getting the latest updates when something changes, ensure that you have the version in the file name, when you update the file also change the version number.

```xml
<configuration>
   <system.webServer>
      <staticContent>
         <clientCache cacheControlMode="UseExpires"
            httpExpires="Tue, 19 Jan 2038 03:14:07 GMT" />
      </staticContent>
   </system.webServer>
</configuration>
```
You can read more about client cache [here](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

### Always keep CSS and JavaScript external
Never add any JavaScript or inline style information within the views. That would regenerate the view each time and you would miss out on the benefits of the above.

Hence always keeps JS and CSS as separate files and add them as links in the view.

Note: Best practice is to add the link to the style file at the top of the view and JS at the bottom of the view file.

### Use Url Compression
Since IIS 7+ allows easy way of compressing the response using gzip protocol and the browser decompresses the response on the client side. This would considerable reduce the network latency while transporting data.

There are two types of compression Static and Dynamic based on the contents. JS, CSS, Images and other static contents would be part of static compression, but the views, data would be come under dynamic compression. You can enable them using the following settings in the configuration file.

```xml
<urlCompression doStaticCompression="true" 
doDynamicCompression="false" />
```
![Compression](/assets/images/compression.png)

above picture shows a simple request to the webserver which implements the dynamic compression. Data transferred over the wire is 4.7 KB after decompressing on the client side the data is 45 KB .

Note: The dynamic compression puts load on the server as each request as to be compressed so use it wisely.

You can read more about setting up the compression [here](http://www.iis.net/configreference/system.webserver/urlcompression)

### Use Output Caching
Use output caching for regularly used views or pages which have no dynamic updates. This can be done using an attribute (OutputCache) to the action in MVC.

More reading [here](http://www.asp.net/mvc/tutorials/older-versions/controllers-and-routing/improving-performance-with-output-caching-cs).

### Use Data Caching
Reduces the database or the disk i/o by caching the regularly used data to in-memory cache. There are many providers in the market and also a default one available with IIS.

### ASP.Net Pipeline Optimization
ASP.net has many http modules waiting for request to be processed and would go through the entire pipeline even if it’s not configured for your application.

All the default modules will be added in the machine.config place in “$WINDOWS$\Microsoft.NET\Framework\$VERSION$\CONFIG” directory. One could improve the performance by removing those modules which you wouldn’t require.

```xml
 <httpModules>
         <!--<span class="code-comment">
         Remove unnecessary Http Modules for faster pipeline 
         </span>-->
         <remove name="Session" />
         <remove name="WindowsAuthentication" />
         <remove name="PassportAuthentication" />
         <remove name="AnonymousIdentification" />
         <remove name="UrlAuthorization" />
         <remove name="FileAuthorization" />
</httpModules>
```

### Avoid Session State
Session states should always be kept really small in size, if we cannot avoid the circumstances, then we should use session as distributed in-memory cache. Never use a database backed session provider.

### Remove Unnecessary HTTP Headers

ASP.Net adds headers that aren’t really necessary to be transmitted over the wire. Such as ‘X-AspNet-Version’ , ‘X-Powered-By’ and many more.

![headers](/assets/images/headers.png)


### Compile in Release mode
Always set the build configuration to release mode for the website. For obvious reasons.

### Turn Tracing off
Tracing is a good functionality, but each of the functionality would add an overhead instead use the asynchronous logging mechanism.

### Async and Await
Since the async controllers are available since the MVC3.0 now we can have non-blocking requests to the webserver which improves the throughput of the requests made. You can read more about this @ http://www.campusmvp.net/blog/async-in-mvc-4.

### HTTP Limitations
By default HTTP protocol doesn’t allow more than two concurrent requests from the same user and those requests are also limited by the browsers

|      |    |   
| ------- |------|
|Firefox 2: | 2|
|Firefox 3+: |6|
|Opera 9.26: |4|
|Opera 12:   |6|
|Safari 3:   |4|
|Safari 5:   |6|
|IE 7:       |2|
|IE 8:       |6|
|IE 10:      |8|
|Chrome:     |6|

But there are scenarios where you would a webserver connecting to a webservice requesting data frequently then this restriction can degrade the performance. .Net has a way to overcome this restriction and allow users to make multiple concurrent calls to the service.

```xml
 <system.net>
      <connectionManagement>
        <!-- Add address from the trusted connection only -->
        <add address="*" maxconnection="100" />
      </connectionManagement>
    </system.net>
```

### Some of the tools worth mentioning

#### YSlow2
Yahoo’s add-in available for most of non-IE browsers. It analyses the web pages against the set of rules by default. You can look at the rules here

![headers](/assets/images/yslow.jpg)

#### Chrome Inspector
Chrome browser’s audit tab allows to run the checks for performance.

![headers](/assets/images/chromebrowser.jpg)

#### Firebug for firefox, IE’s F12 window and Chrome’s element inspector
could be used to track the network utilization and track all the http request made and can gauge which needs your attention.

#### Net Profiler – Available with Visual studio ultimate

#### ANTs profiler

#### Ayande profiler 
http://hibernatingrhinos.com/Products/EFProf

Along with above mentioned checklist/recommendations one should always ensure that the best practices for languages (C#, VB.Net, JavaScript) is followed for the optimum performance of any application.

