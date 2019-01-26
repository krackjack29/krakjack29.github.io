---
title: "Streaming tweets at real time using .Net"
date: 2010-03-19 15:25:18 +530
layout: single
comments: true
categories: 
   - Tech
tags:
   - socialmedia
   - csharp
   - tools
   - howto
---

Twitter has exposed a new set of APIs to stream the tweets from the users worldwide just as we stream a video from any website. This can be used from an application to analyze the sentiment of customers from twitter or even to track certain product keywords which has just been launched. 

Rest API:

URL: [http://stream.twitter.com/1/statuses/filter.json](http://stream.twitter.com/1/statuses/filter.json)

Method: Post

Parameters: count,delimited, follow, track, locations

Returns a stream of [status elements](http://apiwiki.twitter.com/Twitter-REST-API-Method:-statuses%C2%A0show)
 

In the below example i am tracking for CRM, .Net and Salesforce keywords. This returns me a real time stream of tweets containing one of this keywords.

```csharp
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(“http://stream.twitter.com/1/statuses/filter.json&#8221;);

//string postdata = “follow=15670515,19601111,15727368”;
string postdata = “track=CRM,.Net,Salesforce”;
byte[] data = Encoding.UTF8.GetBytes(postdata);
request.Method = “POST”;
request.ContentLength = data.Length;
request.Credentials = new NetworkCredential(“username”, “password”);
request.GetRequestStream().Write(data, 0, data.Length);
request.GetRequestStream().Close();
request.ContentType = “application/x-www-form-urlencoded”;
HttpWebResponse response = null;
try
{
    response = (HttpWebResponse)request.GetResponse();
    new Action<Stream>(BeginRead).BeginInvoke(
  response.GetResponseStream(),
  new AsyncCallback(EndRead),
  null);

}
catch (WebException webEx)
{

}
```

In the “BeginRead” function we can extract the data from the stream and parse the same(data will be in json format, but can be extracted as an xml or atom format by changing the url)

There are many more streaming apis which are exposed by twitter. You can read about them @ [Twitter Streaming API](http://apiwiki.twitter.com/Streaming-API-Documentation)