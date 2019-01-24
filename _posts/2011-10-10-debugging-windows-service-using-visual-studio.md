title: Debugging Windows service using Visual Studio
link: https://pratapgowda.wordpress.com/2011/10/10/debugging-windows-service-using-visual-studio/
author: pratapgowda
description: 
post_id: 84
created: 2011/10/10 19:20:00
created_gmt: 2011/10/10 13:50:00
comment_status: open
post_name: debugging-windows-service-using-visual-studio
status: publish
post_type: post

# Debugging Windows service using Visual Studio

<p>Few of the methods to debug a windows service project are</p>  <p>1. Write a simple windows/console application and move all the logic in the service to other assemblies.</p>  <p>2. In the main function of the service, suspend the thread for a know amount of time and then attach the process to class libraries which you want to debug.</p>  <p>&#160; <div style="display:inline;float:none;margin:0;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:1038cbd6-3c6e-4e17-b521-4faaa4504cf0" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background:#000080;color:#fff;font-family:Verdana, Tahoma, Arial, sans-serif;font-weight:bold;padding:2px 5px;">Code Snippet</div> <div style="background:#ddd;max-height:300px;overflow:auto;"> <ol start="1" style="background:#ffffff;margin:0 0 0 2em;padding:0 0 0 5px;"> <li><span style="color:#808080;">ServiceBase[