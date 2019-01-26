---
title: "Akka.Net - Create actor if not exists"
date: 2017/04/25 13:56:08 +530
layout: single
comments: true
categories: 
   - Tech
tags:
   - .net
   - akka.net
   - csharp
---

If you’d have an actorsystem within an web application, you would have faced the problem of creating an actor with a unique id. Since web is by default stateless it becomes imperative for the actors to be identified by the right names and Uri.

Akka.net doesn’t allow actors to be created with same name, it would throw up an exception. And it misses CreateIfNotExists method on actor creation. So I ended up writing a helper method to check if an actor is already created. If it is, method returns an IActorRef else creates a new actor with the actor name passed.

```csharp
public static async Task<IActorRef> CreateActorIfNotExistsAsync(this ActorSystem actorSystem,
    ActorMetadata actorPath, Props creationProps)
{
    IActorRef actor = null;
    try
    {
        actor = await actorSystem.ActorSelection(actorPath.Path)
            .ResolveOne(TimeSpan.FromSeconds(1));
    }
    catch (ActorNotFoundException)
    {
        actor = actorSystem.ActorOf(creationProps, actorPath.Name);
    }
    return actor;
} 
```

ActorMetadata is a simple class containing two properties i.e. absolute Path of the actor and the name of the actor.

```csharp
public class ActorMetadata
{
    public string Name { get; private set; }
    public string Path { get; private set; }

    public ActorMetadata Parent { get; private set; }
    public ActorMetadata(string name, ActorMetadata parent = null)
    {
        Name = name;
        var parentPath = parent != null ? parent.Path : "/user";
        Path = $"{parentPath}/{Name}";
    }
}
```

Code is shared at [Github](https://github.com/pratapbhaskar/akka-createifnotexists).

Happy coding!!!!!