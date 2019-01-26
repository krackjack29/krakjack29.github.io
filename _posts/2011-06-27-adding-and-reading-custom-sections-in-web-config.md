---
title: Adding and reading custom sections in web.config
date: 2011/06/27 15:24:00 +530
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
There are many scenarios where the programmer would need to add “custom data” in the configuration file other than the connection strings.

In one particular scenario i needed the application keys and secret keys of the Social media in the config file and read the same through the application,

![App config](/assets/images/appconfig.png)

My config file would contain the custom section as shown above, before i could read the same from the application i would need to create a class to read the same.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Configuration;
 
namespace WebRole1.Account
{
    /// <summary>
    ///
    /// </summary>
    public class SocialMediaConfiguration :ConfigurationSection
    {
        [ConfigurationProperty("SocialMedia")]
        public SocialMediaSettingCollection SocialMediaSettings
        {
            get { return this["SocialMedia"] as SocialMediaSettingCollection; }
        }
 
    }
 
    /// <summary>
    ///
    /// </summary>
    public class SocialMediaSetting : ConfigurationElement
    {
        /// <summary>
        /// Gets the name.
        /// </summary>
        /// <value>The name.</value>
        [ConfigurationProperty("name", IsRequired = true, IsKey = true)]
        public string Name
        {
            get { return this["name"] as string; }
        }
 
        /// <summary>
        /// Gets the API key.
        /// </summary>
        /// <value>The API key.</value>
        [ConfigurationProperty("ApiKey", IsRequired = true)]
        public string ApiKey
        {
            get { return this["ApiKey"] as string; }
        }
 
        /// <summary>
        /// Gets the secret key.
        /// </summary>
        /// <value>The secret key.</value>
        [ConfigurationProperty("SecretKey", IsRequired = true)]
        public string SecretKey
        {
            get { return this["SecretKey"] as string; }
        }
 
        /// <summary>
        /// Gets the icon URL.
        /// </summary>
        /// <value>The icon URL.</value>
        [ConfigurationProperty("IconUrl", IsRequired = false)]
        public string IconUrl
        {
            get { return this["IconUrl"] as string; }
        }
    }
 
    public class SocialMediaSettingCollection : ConfigurationElementCollection
    {
        /// <summary>
        /// Gets or sets the <see cref="SocialCrm.SocialMediaSetting"/> at the specified index.
        /// </summary>
        /// <value></value>
        public SocialMediaSetting this[int index]
        {
            get { return this.BaseGet(index) as SocialMediaSetting; }
            set
            {
                if (this.BaseGet(index) != null)
                { base.BaseRemoveAt(index); }
                this.BaseAdd(index, value);  
            }
        }
 
        /// <summary>
        /// Gets or sets the <see cref="SocialCrm.SocialMediaSetting"/> at the specified index.
        /// </summary>
        /// <value></value>
        public SocialMediaSetting this[string key]
        {
            get { return this.BaseGet(key) as SocialMediaSetting; }
        }
        /// <summary>
        /// When overridden in a derived class, creates a new <see cref="T:System.Configuration.ConfigurationElement"/>.
        /// </summary>
        /// <returns>
        /// A new <see cref="T:System.Configuration.ConfigurationElement"/>.
        /// </returns>
        protected override ConfigurationElement CreateNewElement()
        {
            return new SocialMediaSetting();
        }
 
        /// <summary>
        /// Gets the element key for a specified configuration element when overridden in a derived class.
        /// </summary>
        /// <param name="element">The <see cref="T:System.Configuration.ConfigurationElement"/> to return the key for.</param>
        /// <returns>
        /// An <see cref="T:System.Object"/> that acts as the key for the specified <see cref="T:System.Configuration.ConfigurationElement"/>.
        /// </returns>
        protected override object GetElementKey(ConfigurationElement element)
        {
            return ((SocialMediaSetting)element).Name;   
        }
    }
}
```

The class “SocialMediaConfiguration” would allow you to access the appsettings as below

```csharp
SocialMediaConfiguration socialConfig = (SocialMediaConfiguration)ConfigurationManager.GetSection("SocialMediaSettings");
```

Just before we launch our code we need to mention the custom section that we just defined to the web.config file, this can be done using the following tags with the “ConfigSections” tag of the web.config

**Note: This tag as to be the first child tag under the “Configuration” section**

```xml
<configSections>
    <section name="SocialMediaSettings" type="WebRole1.Account.SocialMediaConfiguration"/>
</configSections>
```
You can add many more such tags by inheriting from “ConfigurationSection” class.

Happy coding!!!