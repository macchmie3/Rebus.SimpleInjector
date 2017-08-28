# Rebus.SimpleInjector

[![install from nuget](https://img.shields.io/nuget/v/Rebus.SimpleInjector.svg?style=flat-square)](https://www.nuget.org/packages/Rebus.SimpleInjector)

Provides a SimpleInjector-based container adapter for [Rebus](https://github.com/rebus-org/Rebus).

![](https://raw.githubusercontent.com/rebus-org/Rebus/master/artwork/little_rebusbus2_copy-200x200.png)

---

To configure Rebus to work with your SimpleInjector container, simply call

```csharp
container.ConfigureRebus(
    configurer => configurer
        .(...)
        .Start()
);
```

where the `(...)` is the part usually omitted from the Rebus configuration examples.

A slightly more realistic example (using Serilog, RabbitMQ and SQL Server) could look like this:

```csharp
container.ConfigureRebus(
    configurer => configurer
        .Logging(l => l.Serilog())
		.Transport(t => t.UseRabbitMq("amqp://rebususer:blablasecret@BIGRABBIT01.local", "simpleinjectortest"))
		.Sagas(s => s.StoreInSqlServer("server=SQLMOTEL01.local; database=RebusStuff; trusted_connection=true"))
        .Start()
);
```

The examples shown so far will make the necessary container registrations, but the bus will not be started until either

1. The container resolves the `IBus` instance, or
1. You call the `container.StartBus()` extension method

### So why is it different from all the other container adapters?

Beacuse SimpleInjector is very opinionated about its registration API and Rebus is pretty loose about it :)