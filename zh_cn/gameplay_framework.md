# 游戏框架

> 以下几页将解释虚幻引擎的"游戏框架"中一些最常见的类。它还将提供如何使用这些类的小示例。
>
> 此列表作为框架的介绍。主要是为了确保我可以在纲要的其余部分提及它们。
>
> 列出的示例都需要有关复制的知识。如果您无法遵循示例，请暂时忽略它，直到您阅读有关复制等的章节。
>
> 某些游戏类型可能会以不同的方式使用这些类。以下示例和解释并不是使用类的唯一方法。

## [📄️ AGameMode](gameplay_framework/agamemode.md)

在 4.14 中，AGameMode 类分为 AGameModeBase 和 AGameMode。GameModeBase 的功能较少，因为某些游戏可能不需要旧 AGameMode 类的完整功能列表。

## [📄️ AGameState](gameplay_framework/agamestate.md)

在 4.14 中，GameState 类被分为 AGameStateBase 和 AGameState。GameStateBase 的功能较少，因为某些游戏可能不需要旧 GameState 类的完整功能列表。

## [📄️ APlayerState](gameplay_framework/aplayerstate.md)

APlayerState 类是共享特定玩家信息的最重要的类。它旨在保存有关玩家的当前信息。每个玩家都有自己的 PlayerState。

## [📄️ APawn 和 ACharacter](gameplay_framework/apawn_and_acharacter.md)

APawn 类是玩家控制的"AActor"。大多数时候它是一个人形角色，但也可能是一只猫、飞机、船、方块等。

## [📄️ APlayerController](gameplay_framework/aplayercontroller.md)

APlayerController 类可能是我们遇到的最有趣、最复杂的类。它也是许多客户端逻辑的中心，因为这是客户端实际"拥有"的第一个类。

## [📄️ AHUD](gameplay_framework/ahud.md)

AHUD 类仅在每个客户端上可用，并且可以通过 PlayerController 访问。它将由 PlayerController 自动生成。

## [📄️ UUserWidget (UMG Widget)](gameplay_framework/uuserwidget_umg_widget.md)

UUserWidgets 用于 Epic Games 的 UI 系统，称为 Unreal Motion Graphics。
