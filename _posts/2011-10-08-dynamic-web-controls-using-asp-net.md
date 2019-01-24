title: Dynamic Web Controls using ASP.Net
link: https://pratapgowda.wordpress.com/2011/10/08/dynamic-web-controls-using-asp-net/
author: pratapgowda
description: 
post_id: 83
created: 2011/10/08 16:44:00
created_gmt: 2011/10/08 11:14:00
comment_status: open
post_name: dynamic-web-controls-using-asp-net
status: publish
post_type: post

# Dynamic Web Controls using ASP.Net

<p>&#160;</p>  <p>There are many scenarios where one would have to create dynamic controls based on the logged in user, request etc. Main challenges with the dynamic controls is to save the data from these controls during the postback.&#160; </p>  <p>In this blog we will go through how we create dynamic controls in asp.net without losing the data from these controls during the postback. </p>  <p>I am a windows application developer and was really confused about the asp.net page life cycle but few of the articles mentioned below gave a through understanding of the page cycle, thanks to the authors of these articles. </p>  <p>A Page goes through the following steps each time a request/postback is made</p>  <ol>   <li><strong>Instantiation</strong> – Instantiation of the code behind class, and building the control hierarchy </li>    <li><strong>Initialization</strong> – Page and the controls in the control hierarchy’s init events are triggered </li>    <li><strong>Load View State</strong> – Happens only in Postback, the view state data that had been saved from the previous page visit is loaded and recursively populated into the control hierarchy<code> </code>of the Page </li>    <li><strong>Load Postback Data</strong> – The server control’s LoadPostData() is called </li>    <li><strong>Load</strong> – The Page’s Load event is called </li>    <li><strong>Raise Postback event</strong> </li>    <li><strong>Save View State</strong> </li>    <li><strong>Render Html</strong> </li> </ol>  <p>If you have a closer look at the creating the controls at the load event of the page would never get a chance to load the view state and hence one should create these dynamic controls before the load event i.e. in the Init event of the page.</p>  <p>In an ASP application created using Visual Studio template , the init event is&#160; </p>  <pre class="code"><span style="color:blue;">protected void </span>Page_Init(<span style="color:blue;">object </span>sender, <span style="color:#2b91af;">EventArgs </span>e)</pre>