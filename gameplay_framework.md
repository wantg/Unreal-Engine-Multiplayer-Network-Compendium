# Gameplay Framework

> The following pages will explain some of the most Common Classes in Unreal Engine's "Gameplay Framework". It will also provide small examples of how these classes would be used.
>
> This list serves as an introduction to the framework. Mainly to ensure that I can mention them throughout the rest of the compendium.
>
> The listed examples will all require knowledge about replication. If you can't follow an example just ignore it for now until you read the chapters about replication etc.
>
> Some game genres might use these classes differently. The following examples and explanations aren't the only way to utilize a class.

## [📄️ AGameMode](gameplay_framework/agamemode.md)

With 4.14, the AGameMode Class got split into AGameModeBase and AGameMode. GameModeBase has fewer features because some games might not need the full feature list of the old AGameMode Class.

## [📄️ AGameState](gameplay_framework/agamestate.md)

With 4.14, the GameState Class got split into AGameStateBase and AGameState. GameStateBase has fewer features because some games might not need the full feature list of the old GameState Class.

## [📄️ APlayerState](gameplay_framework/aplayerstate.md)

The class APlayerState is the most important class for shared information about a specific player. It is meant to hold current information about the player. Each player has their PlayerState.

## [📄️ APawn and ACharacter](gameplay_framework/apawn_and_acharacter.md)

The class APawn is the 'AActor' which the player controls. Most of the time it's a humanoid character, but it could also be a cat, plane, ship, block, etc.

## [📄️ APlayerController](gameplay_framework/aplayercontroller.md)

The class APlayerController might be the most interesting and complicated class that we come across. It's also the center for a lot of client logic since this is the first class that the client actually 'owns'.

## [📄️ AHUD](gameplay_framework/ahud.md)

The class AHUD is only available on each client and can be accessed through the PlayerController. It will be automatically spawned by the PlayerController.

## [📄️ UUserWidget (UMG Widget)](gameplay_framework/uuserwidget_umg_widget.md)

UUserWidgets are used in Epic Games' UI System, called Unreal Motion Graphics.
