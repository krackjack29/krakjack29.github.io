---
title: Debugging Windows service using Visual Studio
date: 2011/10/10 19:20:00 +530
layout: single
categories: 
   - .Net
tags:
   - .net
   - csharp
   - howto
---

Few of the methods to debug a windows service project are

* Write a simple windows/console application and move all the logic in the service to other assemblies.

* In the main function of the service, suspend the thread for a know amount of time and then attach the process to class libraries which you want to debug.

```csharp
ServiceBase[] ServicesToRun;
ServicesToRun = new ServiceBase[]
{
    new EMActionServerService()
};
Thread.Sleep(100000);
ServiceBase.Run(ServicesToRun);
```
* Use If directive and write the following code and directly run the service from visual studio –BEST

```csharp
/// <summary>
/// The main entry point for the application.
/// </summary>
static void Main()
{
    #if(!DEBUG)
    ServiceBase[] ServicesToRun;
    ServicesToRun = new ServiceBase[]
    {
        new EMActionServerService()
    };
    ServiceBase.Run(ServicesToRun);
    //Release Code
    #else
    EMActionServerService service = new EMActionServerService();
    service.StartService();
    System.Threading.Thread.Sleep(System.Threading.Timeout.Infinite);       
    #endif
}
```

I found this through an article at [CodeProject](http://www.codeproject.com/KB/dotnet/DebugWinServices.aspx).

I found all the above mentioned 3 types advantageous.

Happy Programming!!!