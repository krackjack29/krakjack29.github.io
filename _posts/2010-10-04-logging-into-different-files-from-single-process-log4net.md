---
title: Logging into different files from single process - Log4Net
date: 2010/10/04 10:58:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - tech
   - howto
   - tools
---

Logging is a very important feature that one would desire for a data/performance intensive applications. Logging helps us in debugging, giving vital and extensive information to the user.

There are many logging techniques that one follow, Event Log, Flat files, SQL Log etc. And also there are many 3rd party libraries which provide a easy configurable logging and one of them we use is [log4net](http://logging.apache.org/log4net/index.html).

All we have to do is edit the config file and voila we are done. For more information just go through their [documentation](http://logging.apache.org/log4net/release/sdk/index.html).

But we had a unique situation where we had ***log data into 2 different files based on the functionality provided by two different .Net Types, both these types are loaded in the same process***. With a little help of googling and trial and error, we were able to resolve the issue using log4Net.

Log4Net provides a tag known as <appender> which defines the log file and its information, and provides a <logger> tag where we can define the type which should use the above defined log file. 

```xml
<log4net>
  <!– Import Utility Config sections–>
  <appender name="ImportUtilityAppender" type="log4net.Appender.RollingFileAppender">
    <file value="ImportLog\ImportUtility-Information Log.log" />
    <layout type="log4net.Layout.PatternLayout">
      <conversionPattern value="%-date – %level – %message%newline" />
    </layout>
  </appender>
  <logger name = "ImportUtilityClassName">
    <level value="INFO" />
    <appender-ref ref="ImportUtilityAppender" />
    <param name="Threshold" value="INFO"/>
  </logger>
  <!– Bulk Delete section–>
  <appender name="BulkDeleteAppender" type="log4net.Appender.RollingFileAppender">
    <file value="BulkDelete\BulkDelete-Information Log.log" />
    <layout type="log4net.Layout.PatternLayout">
      <conversionPattern value="%-date – %level – %message%newline" />
    </layout>
  </appender>
  <logger name = "BulkDeleteClassName">
    <level value="INFO" />
    <appender-ref ref="BulkDeleteAppender" />
    <param name="Threshold" value="INFO"/>
  </logger>
</log4net >
```

Above is the piece of xml that needs to go into the config file. As per the above config file the data from **ImportUtilityClass** would be logged in ImportUtility-Information Log file and bulk delete would be logged into its respective file.