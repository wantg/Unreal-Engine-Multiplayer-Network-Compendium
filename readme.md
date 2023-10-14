# Multiplayer Network Compendium

Original text and copyright: <https://cedric-neukirchen.net/docs/category/multiplayer-network-compendium>

This compendium is meant to give you a good start into multiplayer programming for Unreal Engine.

## [📄️ Introduction](introduction.md)

This Compendium should only be used with a base understanding of the Singleplayer Game Framework of Unreal Engine.

## [📄️ Network in Unreal](network_in_unreal.md)

Unreal Engine uses a standard Server-Client architecture. This means the server is authoritative and all data must be sent from the client to the server first. After which the server validates the data and reacts depending on your code.

## [🗃️ Gameplay Framework](gameplay_framework.md)

7 items

## [📄️ Gameplay Framework + Network](gameplay_framework_in_network.md)

With the previous information about Unreal Engine's Server-Client architecture and the common classes we can divide those into four categories:

## [📄️ Dedicated Server vs Listen Server](dedicated_server_vs_listen_server.md)

Dedicated Server

## [📄️ Replication](replication.md)

What is 'Replication'?

## [📄️ Remote Procedure Calls](remote_procedure_calls.md)

Other ways for Replication are so-called "RPC"s. Short form for "Remote Procedure Call".

## [📄️ Ownership](ownership.md)

Ownership is something very important to understand. You already saw a table that contained entries like "Client-owned Actor".

## [📄️ Actor Relevancy and Priority](actor_relevancy_and_priority.md)

Relevancy

## [📄️ Actor Role and RemoteRole](actor_role_and_remoterole.md)

We have two more important properties for Actor replication.

## [📄️ Traveling in Multiplayer](traveling_in_multiplayer.md)

Non-/Seamless Travel

## [📄️ How to Start a Multiplayer Game](how_to_start_a_multiplayer_game.md)

The easiest way to start a Multiplayer Game is to set the Number of Players, in the Play Drop-Down Menu, to something higher than 1.

## [📄️ Additional Resources](additional_resources.md)

Here is a somewhat unordered list of additional resources that could help you learn
