# 多人网络概要

Original text and copyright: <https://cedric-neukirchen.net/docs/category/multiplayer-network-compendium>

本纲要旨在为您提供虚幻引擎多人游戏编程的良好开端。

## [📄️简介](introduction.md)

本纲要仅应在对虚幻引擎的单人游戏框架有基本了解的情况下使用。

## [📄️ 虚幻中的网络](network_in_unreal.md)

虚幻引擎使用标准的服务器-客户端架构。这意味着服务器是权威的，所有数据必须首先从客户端发送到服务器。之后，服务器验证数据并根据您的代码做出反应。

## [🗃️ 游戏框架](gameplay_framework.md)

7 项目

## [📄️ 游戏框架 + 网络](gameplay_framework_in_network.md)

根据前面关于虚幻引擎的服务器-客户端架构和常用类的信息，我们可以将它们分为四类：

## [📄️专用服务器与监听服务器](dedicated_server_vs_listen_server.md)

专用服务器

## [📄️ 复制](replication.md)

什么是"复制"？

## [📄️ 远程过程调用](remote_procedure_calls.md)

其他复制方式称为"RPC"。"远程过程调用"的缩写形式。

## [📄️ 所有权](ownership.md)

所有权是需要理解的非常重要的事情。您已经看到一个表格，其中包含"客户拥有的 Actor "等条目。

## [📄️ Actor 的相关性和优先级](actor_relevancy_and_priority.md)

关联

## [📄️ Actor 角色和 RemoteRole](actor_role_and_remoterole.md)

对于 Actor 复制，我们还有两个更重要的属性。

## [📄️ 多人旅行](traveling_in_multiplayer.md)

非/无缝旅行

## [📄️ 如何开始多人游戏](how_to_start_a_multiplayer_game.md)

开始多人游戏的最简单方法是在"游戏"下拉菜单中将"玩家数量"设置为大于 1 的值。

## [📄️ 其他资源](additional_resources.md)

以下是一些可以帮助您学习的附加资源的无序列表
