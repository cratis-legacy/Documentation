
# Naming convention

All projects are packaged with the prefix Cratis with additional more specific
purpose appended to the name separated with dots. The naming convention is across
the different programmnig languages and environments supported by Cratis.

## Client

All client specific projects add the .Client purpose to the name, yielding
Cratis.Client.

## NET

All .NET specific projects add the .NET purpose to the name, yielding
Cratis.NET for non-client or Cratis.Client.NET for client purposed packages.

## JavaScript

All JavaScript specific projects add the .JavaScript purpose to the name, yielding
either Cratis.JavaScript for non-client or Cratis.Client.JavaScript for client
purposed packages.

# List of packages

## .NET - Nuget

### Cratis.Core

NOTE: Rename project to Cratis.NET.Core

* Configuration
* Specifications

### Cratis.NET.Tenancy

### Cratis.NET.Resources

* Strings

### Cratis.NET.Localization

* Localization Scoping

### Cratis.NET.Security

### Cratis.NET.Validation

### Cratis.NET.Concepts

### Cratis.NET.Commands

* Command
* CommandCoordinator
* ICanHandleCommands

### Cratis.NET.Events

* ICanProcessEvents

We need to be able to classify events in a way that makes it possible to subscribe
to the events on the client side. The scope varies. 

For instance, not all events should be broadcasted to all clients. Suggest that 
the scope is by default for the client that caused through a command. If it didn't
come as a result of a command, it is a global / broadcasted event. 

For this particular scoping, attributes might be helpful:

    [PublicEvent]
    public class MyEvent : Event
    {
    }


### Cratis.NET.EventSourcing

### Cratis.NET.EventStore

### Cratis.NET.Events.MapReduce

NB: MapReduce is not the right name - have to find a better name for this.

Descriptors for describing how to map events and reduce it down.
e.g.

    public class MyMapReduce : ICanDescribeMapReduce
    {
        public MyMapReduce()
        {
            With<SomeEvent>()
                .When(s=>s.SomeProperty == 42)
                .IdentifiedBy(s=>s.UniqueIdentifier)
                ,Count()
                ,As("NameOfReducedProperty");

            With<SomeEvent>()
                .IdentifiedBy(s=>s.UniqueIdentifier)
                .Sum(s=>s.SomeProperty)
                .As("NameOfReducedProperty");
        }
    }

This should result in an object representation that could also be generated from a
simple JSON or other formats.

Interesting to generate new events:

    public class MyMapReduce : ICanDescribeMapReduce
    {
        public MyMapReduce()
        {
            With<SomeEvent>()
                .When(s=>s.SomeProperty == 42)
                .IdentifiedBy(s=>s.UniqueIdentifier)
                ,Count()
                ,As("NameOfReducedProperty");
                .GenerateNewEvent((e,r) => {
                    // e is the original event
                    // r is the reduced result (dynamic)
                    new OtherEvent {
                        UniqueIdentifier = e.UniqueIdentifier,
                        SomeProperty = e.SomeProperty,
                        Count = r.NameOfReducedProperty
                    };
                });
        }
    }


### Cratis.NET.Domain

* AggregateRoot
* 

### Cratis.NET.Read

* IReadModel
* ReadModelInstanceOf<> - IReadModel

### Cratis.NET.Querying

* IQueryFor<> - IReadModel
* QueryCoordinator
* QueryResult
* QueryProvider
* Query Security
* Query Validation
* Query Paging

### Cratis.NET.ProxyGenerator

### Cratis.NET.Resources.ProxyGenerator

### Cratis.NET.Security.ProxyGenerator

### Cratis.NET.Validation.ProxyGenerator

### Cratis.NET.Commands.ProxyGenerator

### Cratis.NET.Events.ProxyGenerator

### Cratis.NET.Domain.ProxyGenerator

### Cratis.NET.Read.ProxyGenerator

### Cratis.NET.ProxyGenerator.JavaScript

Support ES5 and ES201x


## .NET - Nuget - Technology Specific

### Cratis.NET.SQLServer

### Cratis.NET.Commands.ServiceFabric

### Cratis.NET.Events.ServiceFabric

### Cratis.NET.Events.Azure.EventHub

Azure EventHub

### Cratis.NET.EventStore.Azure.Storage

Uses Azure TableStore in Storage in combination with Redis



## JavaScript - JSPM

Note: Look at naming - prefix of JavaScript does not make sense for the package 
name when the C# one does not have a prefix

### Cratis.JavaScript.Core

* Configuration
* Convention system
* Specifications
* Rules

### Cratis.Client.JavaScript.Dependency.Inversion

Move to non-client specific - this is a JavaScript universal - Make it
Cratis.JavaScript.DependencyInversion (Loose the last .)

### Cratis.JavaScript.Localization

### Cratis.JavaScript.Security

### Cratis.JavaScript.Validation

### Cratis.JavaScript.Commands

### Cratis.JavaScript.Events

* ICanProcessEvents

We need the ability to subscribe to events generated on the server side.


### Cratis.JavaScript.EventSourcing

### Cratis.JavaScript.Domain

### Cratis.JavaScript.Read

### Cratis.JavaScript.Querying

### Cratis.Client.JavaScript.Core

* ValueConverter
* TypeConverter
* MarkupExtension
* DependencyProperty
* Region
* Operation
* Trigger
* Action
* Behavior
