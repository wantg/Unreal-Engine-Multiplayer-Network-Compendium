# AHUD

AHUD 类仅在每个客户端上可用，并且可以通过 PlayerController 访问。它将由 PlayerController 自动生成。

在 UMG（虚幻动态图形）发布之前，AHUD 类已用于在客户端视口中绘制文本、纹理等。

如今 UserWidgets 99% 的时间都取代了 HUD 类。

您仍然可以使用 AHUD 类进行调试，或者可能有一个隔离区域来管理创建、显示、隐藏和销毁 UserWidget。

由于 HUD 不直接链接到多人游戏，因此示例仅显示单人游戏逻辑，因此我们将在本课程中跳过它们。
