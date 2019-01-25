---
title: "Unit Testing Cloud Storage (Tables and Blobs)"
date: 2013/10/01 13:41:00 +530
layout: single
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - unittesting
---
>“Without valid unit tests any code that’s written has no value as its not verifiable. And if your code isn’t testable (unit) there is a major design flaw that you would regret in not so far future. “

Ok those were the things i learnt over a period of years when i wasn’t writing unit tests Smile . Yes unit testing is important and how do we test the code that stores, loads and manipulates data from data-source esp. in our case Azure Storage (table and blobs).

There are ways to stub the storage services, one would be to write a new in-memory stub which provides all the APIs that Azure would provide , but that would be time consuming and who would write unit tests for those heavy stubs ?

Instead Microsoft along with SDKs gives you the emulators for storage and computing which we can use them for the unit testing.

While unit testing we would want our emulator to be running and  reset based on the structuring of the unit tests, two approaches would be to reset storage for each fixture of unit test or reset them for each unit test.

There are few attributes out there in Microsoft.VisualStudio.TestTools.UnitTesting namespace which are very useful in these type of scenarios.

| Attribute     | Description| Signature of methods  Within a TestClass  |
| ------------- |:-------------|:-----|
|AssemblyInitializeAttribute|Called before the tests in an assembly is loaded|public static void MethodName(TestContext context)|
|AssemblyCleanupAttribute|Called after the tests in an assembly is completed its execution|public static void MethodName()|
|ClassInitializeAttribute|Called before tests in a TestClass is loaded|	public static void MethodName(TestContext context)|
|ClassCleanupAttribute| Called after tests in a TestClass is completed|	public static void MethodName()|
|TestInitializeAttribute|Called before executing each test|public void MethodName()|
|TestCleanupAttribute|Called after executing each test|public void MethodName()|

My way of unit testing was that i assume my TestClass would need a clean storage and i clean (reset) the storage one all the tests in my class is complete.

```csharp
[ClassInitialize]
public static void InitializeClass(TestContext context)
{
   ProcessStartInfo devStorageProcessInfo = new ProcessStartInfo();
   devStorageProcessInfo.Arguments = "/devstore:start";
   devStorageProcessInfo.FileName = @"C:\Program Files\Microsoft SDKs\Windows Azure\Emulator\csrun.exe"; 
   //This is for windows Azure SDK 2.1 MS can change this anytime
   var processCount = Process.GetProcesses().Where(a => a.ProcessName.Contains("DSService")).Count();
   if (processCount == 0)
   {
      Process process = new Process();
      process.StartInfo = devStorageProcessInfo;
      process.Start();
      process.WaitForExit();
   }
}

[ClassCleanup]
public static void ResetStorageAfterTestExecution()
{
   CloudTableClient tableClient = CloudStorageAccount.DevelopmentStorageAccount.CreateCloudTableClient();
   IEnumerable<CloudTable> listOfTables = tableClient.ListTables();
   foreach (CloudTable table in listOfTables)
   {
      table.Delete(); //Delete the tables to reset the data
   }
   //You could do the same with the blobs and Queues to reset the storage
}
Now when my test fixture from that class as to execute it checks if the local emulator is exec
```

Now when my test fixture from that class as to execute it checks if the local emulator is executing, if not it would start the instance. And once my tests in that class are executed i reset the storage so that the next class (tests) in line could do there bit of breaking code.

Instead of writing this code in each class, you could create a class out of it and make them static functions and call them as you want.

Happy clean coding !!!!