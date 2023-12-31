# 多人游戏中的旅行

## 非/无缝旅行​

无缝旅行和非无缝旅行之间的区别很简单。

无缝出行是一种非阻塞操作，而非无缝则将是阻塞调用。

客户端的非无缝旅行意味着他们与服务器断开连接，然后重新连接到同一服务器，这将准备好加载新地图。

Epic 建议尽可能经常使用无缝旅行，因为这将带来更流畅的体验，并避免重新连接过程中可能出现的任何问题。

非无缝旅行必须通过三种方式进行：

- 第一次加载地图时
- 当第一次作为客户端连接到服务器时
- 当您想要结束多人比赛并开始新的比赛时

## 主要出行功能​

驱动出行的三大主要功能：

### UEngine::浏览器​

- 就像加载新地图时硬重置一样

- 始终会带来"非无缝"的旅行

- 将导致服务器在前往目的地地图之前断开当前客户端的连接

- 客户端将与当前服务器断开连接

- 专用服务器无法前往其他服务器，因此地图必须是本地的（不能是 URL）

### UWorld::服务器旅行​

- 仅适用于服务器

- 将使服务器跳转到新的世界/级别

- 所有连接的客户端都会跟随

- 这是多人游戏从一个地图到另一个地图的方式，服务器是负责调用此函数的人

- 服务器将为所有连接的客户端玩家调用"APlayerController::ClientTravel"

### APlayerController::ClientTravel​

- 如果由客户端调用，将前往新服务器

- 如果由服务器调用，将指示特定客户端前往新地图（但保持与当前服务器的连接）

## 实现无缝旅行​

无缝旅行配有过渡地图。这是通过"UGameMapsSettings::TransitionMap"属性进行配置的。
默认情况下该属性为空。如果您的游戏将此属性留空，则会为其创建一个空地图。

过渡地图存在的原因是必须始终加载一个世界（其中包含地图），因此我们无法在加载新地图之前释放旧地图。
由于 Map 可能非常大，因此将旧 Map 和新 Map 同时存储在内存中是个坏主意，因此这就是 Transition Map 的用武之地。

设置好过渡地图后，将"AGameMode::bUseSeamlessTravel"设置为 true，从那里开始，无缝旅行应该可以工作了！

![无缝旅行设置](../images/image-11.png)

这也可以通过游戏模式蓝图和"地图和节点"选项卡中的项目设置进行设置。

![项目地图设置](../images/image-12.png)

## 坚持 Actor /无缝旅行​

使用无缝旅行时，可以将 Actor 从当前关卡转移（保留）到新关卡。这对于某些 Actor 很有用，例如库存物品、玩家等。

默认情况下，这些 Actor 会自动持久化：

- GameMode Actor（仅限服务器）
  - 通过"AGameMode::GetSeamlessTravelActorList"进一步添加的任何 Actor

- 具有有效 PlayerState 的所有控制器（仅限服务器）

- 所有玩家控制器（仅限服务器）

- 所有本地玩家控制器（服务器和客户端）
  - 通过在本地 PlayerController 上调用的"APlayerController::GetSeamlessTravelActorList"进一步添加的任何 Actor

以下是执行"无缝"旅行时的一般流程：

1. 标记将持续到过渡级别的 Actor（参见上文）

2. 进入过渡阶段

3. 标记将持续到最终关卡的 Actor（参见上文）

4. 前往最终关卡
