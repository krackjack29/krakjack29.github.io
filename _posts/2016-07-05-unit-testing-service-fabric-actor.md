---
title: Unit testing Service Fabric Actor
date: 2016/07/05 18:06:00 +530
layout: single
comments: true
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - servicefabric
   - howto
   - unittest
---

Once you create a Service Fabric Application and notice the base class “Actor” has no public constructor

```csharp
#region Assembly Microsoft.ServiceFabric.Actors, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
#endregion
 
using System.Threading.Tasks;
 
namespace Microsoft.ServiceFabric.Actors.Runtime
{
    public abstract class Actor : ActorBase
    {
        protected Actor();
        public IActorStateManager StateManager { get; }
        protected Task SaveStateAsync();
    }
}
```

The StateManager property would be the one that you would use many times to save and load the state into the actor and definitely you would want to mock that instance.

Lets take the sample actor which sends an email to reading the state from the StateManager.

```csharp
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Actors.Runtime;
using SampleActor.Interfaces;
 
namespace SampleActor
{
    [StatePersistence(StatePersistence.Persisted)]
    internal class UserActor : Actor, ISampleActor
    {
        private IEmailService emailService;
        public UserActor(IEmailService emailService)
        {
            this.emailService = emailService;
        }
        async Task ISampleActor.RegisterUser(string fullName, string emailAddress)
        {
            await this.StateManager.AddStateAsync<string>("UserEmail", emailAddress);
            await this.StateManager.AddStateAsync<string>("userFullName", fullName);
        }
 
        async Task ISampleActor.SendReminderEmail()
        {
            var userEMail = await this.StateManager.GetStateAsync<string>("UserEmail");
            var userFullName = await this.StateManager.GetStateAsync<string>("userFullName");
 
            await this.emailService.SendEmail("Reminder!!", userEMail, userFullName);
        }
    }
}
```

without the Statemanager mocked we cannot unit test this Actor class. One of the ways that I overcame with this problem is to use shadowing, following is the modified code piece to inject StateManager

```csharp
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Actors;
using Microsoft.ServiceFabric.Actors.Runtime;
using SampleActor.Interfaces;
 
namespace SampleActor
{
    [StatePersistence(StatePersistence.Persisted)]
    internal class UserActor : Actor, ISampleActor
    {
        private readonly IActorStateManager stateManager;
        private readonly ActorId id;
        private IEmailService emailService;
 
        public new IActorStateManager StateManager { get { return stateManager ?? base.StateManager; } }
        public new ActorId Id { get { return id ?? base.Id; } }
        public UserActor(IEmailService emailService,
            IActorStateManager stateManager = null,
            ActorId id = null)
        {
            this.emailService = emailService;
            this.stateManager = stateManager;
            this.id = id;
        }
 
        async Task ISampleActor.RegisterUser(string fullName, string emailAddress)
        {
            await this.StateManager.AddStateAsync<string>("UserEmail", emailAddress);
            await this.StateManager.AddStateAsync<string>("userFullName", fullName);
        }
        async Task ISampleActor.SendReminderEmail()
        {
            var userEMail = await this.StateManager.GetStateAsync<string>("UserEmail");
            var userFullName = await this.StateManager.GetStateAsync<string>("userFullName");
 
            await this.emailService.SendEmail("Reminder!!", userEMail, userFullName);
        }
    }
}
```

With the above implementation once can inject a mock for IActorStateManager as well as the Id property (its only a getter in the Actor class).

### Dependency Injection in Actors
The actor service is registered in the main method in Program.cs, this is the place where the actor is constructed. One can use any dependency injection containers to register the dependencies or can instantiated the dependencies and pass as parameters.

```csharp
using System;
using System.Threading;
using Microsoft.ServiceFabric.Actors.Runtime;
 
namespace SampleActor
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                var emailService = new EmailService();
    //inject email service instance to the UserActor
                ActorRuntime.RegisterActorAsync<UserActor>(
                   (context, actorType) => new ActorService(context, actorType, () => new UserActor(emailService))).GetAwaiter().GetResult();
 
                Thread.Sleep(Timeout.Infinite);
            }
            catch (Exception e)
            {
                ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```
Note: The blog was written with ServiceFabric 2.1.150 in mind, one of the most requested feature is to make Actor more unit testable, maybe in future versions of Servicefabric Microsoft might acknowledge that request.

Happy Coding!!!