# Network in Unreal

Unreal Engine uses a standard Server-Client architecture. This means the server is authoritative and all data must be sent from the client to the server first. After which the server validates the data and reacts depending on your code.

## A Small Example

When you move your character as a client in a multiplayer match, you don't move your character by yourself but tell the server that you want to move it. The server then updates the transform of the character for everyone else, including you.

> INFO
>
> To prevent a feeling of "lag" for the local client, programmers often, in addition, let the local client directly control their character -- although the server still might override the character's Location when the client starts cheating! This means the client will (almost) never 'talk' to other clients directly.

## Another Example​

When sending a chat message to another client you are sending it to the server first, which then passes it to the client you wanted to reach. This could also be a team, guild, group, etc.

> IMPORTANT
>
> Never trust the client! Trusting the client here means you don't test the client's actions before executing them.
>
> This will allow them to cheat!
>
> A simple example would be firing a weapon: Make sure to test, on the server, if the client has the required amount of ammo and is allowed to shoot instead of directly processing the shot!
