# Dedicated Server vs Listen Server

## Dedicated Server​

A Dedicated Server is a standalone server that does NOT require a player to host.

It runs separated from the game client and is mostly used to have a server running that players can always join/leave, without the server shutting down with them.

Dedicated Servers can be compiled for Windows and Linux and can be run on Cloud Servers that players can connect to via a fixed IP-Address.

Dedicated Servers have no visual part, so they don't need a UI, nor do they have a PlayerController. They also have no Character or similar representing them in the Game.

## Listen Server​

A Listen-Server is a server that is also a client.

Due to also being a client, the Listen-Server needs UI and has a PlayerController, which represents the client part. Getting 'PlayerController(0)' on a Listen-Server will return the PlayerController of that very client.

Since the Listen-Server runs on the client itself, the IP that others need to connect to is the one of the clients. Compared to the Dedicated Server, this often comes with the problem of players NOT having a static IP.

However, using an OnlineSubsystem (explained later), the changing IP problem can be solved.
