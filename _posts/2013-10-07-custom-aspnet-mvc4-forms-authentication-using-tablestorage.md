---
title: Custom ASP.Net MVC4 Forms Authentication using TableStorage
date: 2013/10/07 18:16:00 +530
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

As part of MVC4 Microsoft released new authentication provider([WebMatrix.WebData.ExtendedMembershipProvider](http://msdn.microsoft.com/en-us/library/webmatrix.webdata.extendedmembershipprovider(v=vs.111).aspx)) support open authentication and new entity framework.

If we have to write custom authentication provider we would have to derive from this new abstract class which has around 20-30 methods to be implemented. I am going to list down few important ones and when it is used.

### public virtual void **Initialize** (string name, NameValueCollection config)
This is the entry point for your provider, the config parameter, which contains the name of provider, connection string, password attempts and so on, is filled in by the framework from your webconfig.
```xml
<membership defaultProvider="TableStorageMembershipProvider">
   <providers>
      <add name="TableStorageMembershipProvider" 
      type="Prani.Authentication.Provider.TableStorageMembershipProvider,Prani.Authentication.Provider, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" 
      connectionStringName="StorageConnectionName" 
      enablePasswordRetrieval="false" 
      enablePasswordReset="true" 
      requiresQuestionAndAnswer="false" 
      requiresUniqueEmail="true" 
      maxInvalidPasswordAttempts="3" 
      minRequiredPasswordLength="6" 
      minRequiredNonalphanumericCharacters="1" 
      passwordAttemptWindow="10" applicationName="/" />
   </providers>
</membership>
```
above is sample configuration that one needs to add for the custom provider. All the attributes within the add tag would be taken as parameter to the Initialize method.

### public override string **CreateUserAndAccount**(string userName, string password, bool requireConfirmation, IDictionary<string, object> values) 
This is the method called when user is registered. The field values other than username and the password are sent within the dictionary  as values and the code could frame into a query and execute the same.

### public override bool **ValidateUser**(string username, string password)
Its called when user logs in. Responsibility of this function is to verify whether the user name and password are valid with what is stored within the data store.

## Entities for Authentication

By default MVC4 uses SQL database for backend and stores authentication data into SQL tables, but in tablestorage we need to have entities. Following are the entities that are required for authentication.

* UserProfile  : Entity holding the username, userid and password (PK: “SomeText”, RK : Username)
* WebpagesMembership : Entity holding the user information, including last login date, last incorrect password, security question and answer etc. (PK : “SomeText”, RK : Username)
* WebpagesOAuthMembership : Open authentication provider details. (PK : Provider+ProviderUserId, RK: Username”")

So create these classes with ‘TableEntity’ as the base class and pass the PK and RK as mentioned.

![Table Auth](/assets/images/tableauth.jpg)

## TableStorageMembershipProvider

Our provider must implement “ExtendedMembershipProvider” class and for programming azure storage go through my other blogs (Repository Pattern – Part 1 and Part 2)

### Initialize 
```csharp
public override void Initialize(string name, NameValueCollection config)
{
   if (config == null)
      throw new ArgumentNullException("config");
   if (string.IsNullOrEmpty(name))
   {
       name = "TableStorageMembershipProvider";
   }
   if (string.IsNullOrEmpty(config["description"]))
   {
      config.Remove("description");
      config.Add("description", "Table Storage Extended Membership Provider");
   }
   base.Initialize(name, config);

   this.ApplicationName = Helper.GetValueOrDefault(config, "applicationName", o => o.ToString(), "PraniSoft");

   this._connectionName = Helper.GetValueOrDefault(config, "connectionStringName", o => o.ToString(), string.Empty);
   this._entityStoreFactory = new AzureEntityStoreFactory(this._connectionName);

   this._webpagesMembershipRepository = this._entityStoreFactory.GetEntityStore<WebpagesMembership>();
   this._userProfileRepository = this._entityStoreFactory.GetEntityStore<UserProfile>();

   this._enablePasswordRetrival = Helper.GetValueOrDefault(config, "enablePasswordRetrieval", Convert.ToBoolean, false);
   this._enablePasswordReset = Helper.GetValueOrDefault(config, "enablePasswordReset", Convert.ToBoolean, true);
   this._requireUniqueEmail = Helper.GetValueOrDefault(config, "requiresUniqueEmail", Convert.ToBoolean, true);
   this._maxPasswordAttempts = Helper.GetValueOrDefault(config, "maxInvalidPasswordAttempts", Convert.ToInt32, 3);
   this._passwordAttemptWindow = Helper.GetValueOrDefault(config, "passwordAttemptWindow", Convert.ToInt32, 10);
   this._passwordFormat = Helper.GetValueOrDefault(config, "passwordFormat", o =>
   {
      MembershipPasswordFormat format;
      return Enum.TryParse(o.ToString(), true, out format) ? format : MembershipPasswordFormat.Hashed;
   }, MembershipPasswordFormat.Hashed);
   this._minRequiredPasswordLength = Helper.GetValueOrDefault(config, "minRequiredPasswordLength", Convert.ToInt32, 6);
   this._minRequiredNonAlphanumericCharacters = Helper.GetValueOrDefault(config, "minRequiredNonalphanumericCharacters",
                                                                          Convert.ToInt32, 1);
   this._passwordStrengthRegularExpression = Helper.GetValueOrDefault(config, "passwordStrengthRegularExpression",
                                                                       o => o.ToString(), string.Empty);
   this._hashAlgorithmType = Helper.GetValueOrDefault(config, "hashAlgorithmType", o => o.ToString(), "SHA1");

}
```

### CreateUserOrUpdateAccount
```csharp
public override string CreateUserAndAccount(string userName, string password, bool requireConfirmation, IDictionary<string, object> values)
{
   if (string.IsNullOrWhiteSpace(userName))
      throw new MembershipCreateUserException(MembershipCreateStatus.InvalidUserName);

   if (string.IsNullOrWhiteSpace(password))
      throw new MembershipCreateUserException(MembershipCreateStatus.InvalidPassword);

   var userProfileRepository = this._userProfileRepository;
   var usersWithSameUserName = userProfileRepository.Get(new UserProfileReadByPartitionKeyAndRowKey(userName));

   if (usersWithSameUserName.Count() > 0)
      throw new MembershipCreateUserException(MembershipCreateStatus.DuplicateUserName);

   password = Crypto.HashPassword(password);

   var userModel = new UserProfile(userName, password);
   userProfileRepository.Update(userModel);
   userProfileRepository.SubmitSynchronously();

   string token = string.Empty;
   if (requireConfirmation)
      token = GenerateToken();

   var webpagesMemberShipModel = new WebpagesMembership(userName)
   {
      ConfirmationToken = token,
      CreateDate = System.DateTime.UtcNow,
      IsConfirmed = !requireConfirmation,
      PasswordChangedDate = DateTime.UtcNow,
      Password = password
   };
   this._webpagesMembershipRepository.Update(webpagesMemberShipModel);
   this._webpagesMembershipRepository.SubmitSynchronously();

   return token;
}
```
you can download code here at [TableStorageMembershipProvider](http://sdrv.ms/17MYE08).

## Open Authentication and External Login Provider
MVC4 template by default provides an easier way of managing the external open authentication providers such as Facebook, Google, Microsoft and Twitter. You can read about using them [here](http://www.asp.net/mvc/tutorials/security/using-oauth-providers-with-mvc).

Important methods that needs to be implemented for OAuth to work are

```csharp
//Saves the provider and the provideruserId into WebpagesOAuthMembership entity
public override void CreateOrUpdateOAuthAccount(string provider, string providerUserId, string userName)
//Removes the associated external login for the user
public override void DeleteOAuthAccount(string provider, string providerUserId)
//Gets all the associated login providers for the user
public override ICollection<OAuthAccountData> GetAccountsForUser(string userName)
//Used internally by OAuthWebSecurity static class to get UserId
public override int GetUserIdFromOAuth(string provider, string providerUserId)
//Used internally by OAuthWebSecurity static class to get username with an id
public override string GetUserNameFromId(int userId)
```
You can see the implementation at the bottom of the TableStorageMembershipProvider class file.

## How to USE
Create an MVC4 internet application and in the web.config add the above mentioned tag in the web.config and you wouldnt have to change a line of code in your MVC application.

Happy Coding!!!!