---
title: Dynamic Web Controls using ASP.Net
date: 2011/10/08 16:44:00 +530
layout: single
categories: 
   - .Net
tags:
   - .net
   - csharp
   - web
   - asp.net
---

There are many scenarios where one would have to create dynamic controls based on the logged in user, request etc. Main challenges with the dynamic controls is to save the data from these controls during the postback. 

In this blog we will go through how we create dynamic controls in asp.net without losing the data from these controls during the postback.

I am a windows application developer and was really confused about the asp.net page life cycle but few of the articles mentioned below gave a through understanding of the page cycle, thanks to the authors of these articles.

A Page goes through the following steps each time a request/postback is made

1. **Instantiation** – Instantiation of the code behind class, and building the control hierarchy.
2. **Initialization** – Page and the controls in the control hierarchy’s init events are triggered.
3. **Load View State** – Happens only in Postback, the view state data that had been saved from the previous page visit is loaded and recursively populated into the control hierarchy of the Page.
4. **Load Postback Data** – The server control’s LoadPostData() is called.
5. **Load** – The Page’s Load event is called
6. **Raise Postback event**
7. **Save View State**
8. **Render Html**

If you have a closer look at the creating the controls at the load event of the page would never get a chance to load the view state and hence one should create these dynamic controls before the load event i.e. in the Init event of the page.

In an ASP application created using Visual Studio template , the init event is 

```csharp
protected void Page_Init(object sender, EventArgs e)
```
The sample code mentioned below creates two controls dynamically (a textbox and a checkbox) and gets the data after the postback from the button click event

```html
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="DynamicControls_And_ViewState.Default" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title></title>
</head>
<body>
    <form id="form1" runat="server" enableviewstate="true">
    <div>
        <asp:Panel runat="server" ID="MyPanel">
        </asp:Panel>
        <asp:Button runat="server" ID="MyButton" Text="Button" OnClick="MyButton_Click" />
        <br />
        <asp:Label runat="server" ID="MessageLabel"></asp:Label>
    </div>
    </form>
</body>
</html>
```

In the Default.aspx.cs or the Default class the implementation would be
```csharp
using System;
using System.Collections.Generic;
using System.Web.UI;
using System.Web.UI.WebControls;
 
namespace DynamicControls_And_ViewState
{
    public partial class Default : System.Web.UI.Page
    {
        Table tab = new Table();
        Dictionary<string, Control> controlDictionaryMap = new Dictionary<string, Control>(); 
        protected void Page_Init(object sender, EventArgs e)
        {
            tab.ID = "myTab";
 
            TableRow row = new TableRow();
            TableCell cell = new TableCell();
 
            TextBox tb = new TextBox();
            tb.ClientIDMode = System.Web.UI.ClientIDMode.Static;
            tb.ID = "myTextBox";
            controlDictionaryMap.Add("myTextBox", tb);
             cell.Controls.Add(tb);
            row.Cells.Add(cell);
            tab.Rows.Add(row);
 
 
            TableRow row1 = new TableRow();
            TableCell cell1 = new TableCell();
 
            CheckBox cb = new CheckBox();
            cb.ClientIDMode = System.Web.UI.ClientIDMode.Static;
            cb.ID = "myCheckBox";
            controlDictionaryMap.Add("myCheckBox", cb);
               cell1.Controls.Add(cb);
            row1.Cells.Add(cell1);
            tab.Rows.Add(row1);
 
            this.MyPanel.Controls.Add(tab);
        }
       
        protected void Page_Load(object sender, EventArgs e)
        {           
        }
        
        protected void MyButton_Click(object sender, EventArgs e)
        {
            string text = ((TextBox)controlDictionaryMap["myTextBox"]).Text;
            bool check = ((CheckBox)controlDictionaryMap["myCheckBox"]).Checked; 
            this.MessageLabel.Text = string.Format("You typed {0} in the dynamic text box and status of checkbox {1}", text,check.ToString()  );
        }
 
        protected void Button1_Click(object sender, EventArgs e)
        {
        }
    }
}
```

![dynamic web](/assets/images/dynamicweb.png)

Above is the output after the button is clicked

Happy Programming!!!!
You can download the source code [here](http://www.4shared.com/file/Dc77ma32/DynamicControls_And_ViewState.html)

### References
[MSDN](http://msdn.microsoft.com/en-us/library/ms972976.aspx) and [4guysfromrolla](http://www.4guysfromrolla.com/articles/092904-1.aspx)