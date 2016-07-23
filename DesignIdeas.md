# Dependency Inversion - conventions

Configurable conventions based on naming. 
In C# this would typically be mapping between a type and what are its implementation.
The type is usually an interface or an abstract type, but in reality this could
be anything. 

The way we could do this is to define mapping on a string level. 

E.g.

"{type}" => "{type}"
"{interface}" => "{type}" // Assuming IFoo for interface, Foo for type
"{interface}" => "{type}.{executionContext.language.shortCode}"
"{namespace}.{interface}" => "{namespace}.**.{type}.{language.shortCode}"
"{namespace}.{interface}" => "{namespace}.{tenant.name}.{type}.{language.shortCode}"

The {tenant} and {language} variables are expanded automatically relative to the
ExecutionContext. Anything from the ExecutionContext can be used. 
Any of the values expanded from {} is considered a value provider. 
This should expandable - but they should also be unique in naming. 
Otherwise ambiguity would occur.

The same technique can be used in JavaScript as well. Namespace is available 
as metadata to all types, mapped back from the folder structure to the folder
to namespace mapped conventions.

The conventions will be attempted mapped sequentially. If there is no match, it
moves on in the convention list.

At the core of this sits a matching engine that can be reused for other purposes.
The matching engine uses the value expansion system that knows about the 
value providers. Call it: Value interpolation

# Value Interpolation

As with string interpolation in C# and JavaScript, we have the value interpolation
system. Used among other places in the matching system. 

It supports the usage of value providers for its interpolation capabilities. 

In addition to this it supports formatting. 

{value} will expand to whatever the "value" value provider will do. 
Interpolation is done in a certain context. For instance when used during the
dependency inversion convention system, the context would contain the service type.

Parameters can be passed into the interpolation expression:

{date:MMYYYY} - the date value provider would be provided the parameters. 
You can have more than one parameter, separated by |

{something:PARAM1|PARAM2}


# Pipeline

We need the ability to define pipelines - a well defined chain of command in which
ordering matter to a higher level. 

For instance - Command pipeline: 

Command -> Security Check -> Validation Check -> Business Rules Check

Query:

Query -> Security Check -> Validation Check -> Business Rules Check

# Commands -> AggregateRoot -> Events - Azure style

Assumptions:
* Service Fabric
* EventHub

Inside Service Fabric a Command would be dealt with by the CommandCoordinator. 
It would delegate to a CommandHandler that would invoke an AggregateRoot.
The AggregateRoot being an EventSource would generate one or more Events. 
The AggregateRoot would be invoked through an Actor proxy inside Service Fabric.
Any state for the AggregateRoot would be held inside the Actor proxy and the
AggregateRoot would also be resumed with this state. 

Events coming from the AggregateRoot will then be published to an EventHub.

A Stateless Service for processing events in the EventHub will take any event and
store it in the EventStore

A Stateless Service for processing events in the EventHub will take any event and 
call any Event processors. An EventProcessor is wrapped in an Actor proxy holding
the state for the EventProcessor - typically what the last Event it processed was.

# Client Notifications

At the heart of connectivity from the clients to the server sits something like
SignalR or similar abstraction. 

## Query

Queries sitting in the client creates a dependency graph that the server can use
to notify the correct client when changes occur that changes the results of an 
affected query.