# Actor Role and RemoteRole

We have two more important properties for Actor replication.

These two properties tell you:

- Who has authority over the Actor
- Whether the Actor is replicated or not
- The mode of the replication

The first thing we want to determine is who has authority over a specific Actor.

To determine if the currently running instance of the engine has authority, check if the role property is ROLE_Authority.
If it is, then this instance of the engine is in charge of this Actor (whether it's replicated or not!).

> INFO
>
> This is not the same as Ownership!

## Role/RemoteRole Reversal​

Role and RemoteRole could be reversed depending on who is inspecting these values.

For example, if on the server you have this configuration:

- Role == Role_Authority

- RemoteRole = ROLE_SimulatedProxy

Then the client would see it as this:

- Role == ROLE_SimulatedProxy

- RemoteRole == ROLE_Authority

This makes sense since the server is in charge of the Actor and replicating it to clients.
The clients are just supposed to receive updates and simulate the Actor between updates.

## Mode of Replication​

The server doesn't update Actors every update. This would take way too much bandwidth and CPU resources. Instead, the server will replicate Actors at a frequency specified on the 'AActor::NetUpdateFrequency' property.

This means that some time will have passed on the client between Actor updates. This could result in Actors looking sporadic or choppy in their movement. To compensate for this, the client will simulate the Actor between updates.

There are currently two types of simulation that occur:

ROLE_SimulatedProxy

and

ROLE_AutonomousProxy

### ROLE_SimulatedProxy​

This is the standard simulation path and is generally based on extrapolating movement based on the last known velocity.

When the server sends an update for a particular Actor, the client will adjust its position toward the new location, and then in between updates, the client will continue to move the Actor based on the most recent velocity sent from the server.

Simulating using the last known velocity is just one example of how general simulation works.

Nothing stopping you from writing custom code to use some other information to extrapolate between server updates.

### ROLE_AutonomousProxy​

This is generally only used on Actors that are possessed by PlayerControllers.

It just means that this Actor is receiving inputs from a human controller, so when we extrapolate, we have a bit more information and can use actual human inputs to fill in the missing info (rather than extrapolating based on the last known velocity).
