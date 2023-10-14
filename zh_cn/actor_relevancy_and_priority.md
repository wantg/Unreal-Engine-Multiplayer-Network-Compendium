# Actor 的相关性和优先级

## 相关性

什么是"相关性"以及为什么我们需要它？

想象一下，有一款游戏的关卡/地图足够大，玩家可能会被其他玩家视为"不重要"。

如果玩家"A"彼此相距数英里，为什么他们需要从玩家"B"获取网络更新？

为了提高带宽，虚幻引擎的网络代码可以使服务器仅告知客户端有关客户端相关集中的 Actor 的信息。

Unreal（按顺序）应用以下规则来确定玩家的相关 Actor 集。这些测试在虚拟函数"AActor::IsNetRelevantFor()"中实现。

1. 如果 Actor 被标记为"bAlwaysRelevant"，由 Pawn 或 PlayerController 所有，是 Pawn，或者 Pawn 是某些动作（如噪音或损坏）的煽动者，则它是相关的

2. 如果Actor被标记为'bNetUserOwnerRelevancy'并且有一个Owner，则使用Owner的相关性

3. 如果Actor被标记为'bOnlyRelevantToOwner'，并且没有通过第一个检查，那么它是不相关的

4. 如果Actor附加到另一个Actor的Skeleton上，则其相关性由其基础的相关性决定

5. 如果 Actor 被隐藏（'bHidden == true'）并且根组件没有碰撞，那么 Actor 不相关
    - 如果没有根组件，"AActor::IsNetRelevantFor()"将记录警告并询问 Actor 是否应设置为"bAlwaysRelevant = true"

6. 如果"AGameNetworkManager"设置为使用基于距离的相关性，则如果 Actor 比净剔除距离更近，则该 Actor 是相关的

> 信息
>
> Pawn 和 PlayerController 重写 'AActor::IsNetRelevantFor()' 并因此具有不同的相关性条件。

## 优先级

Unreal 使用一种负载平衡技术，对所有 Actor 进行优先级排序，并根据对游戏玩法的重要性为每个 Actor 提供公平的带宽份额。

Actor 有一个名为"NetPriority"的浮点变量。该数字越高，Actor 相对于其他人获得的带宽就越多。
"NetPriority"为 2.0 的 Actor 的更新频率是"NetPriority"为 1.0 的 Actor 的两倍。

与优先级相关的唯一重要的是它们的比例。显然，您无法通过增加所有优先级来提高虚幻的网络性能。

Actor 的当前优先级是使用虚拟函数"AActor::GetNetPriority()"计算的。
为了避免饥饿，"AActor::GetNetPriority()"将"NetPriority"乘以自上次复制 Actor 以来的时间。

"GetNetPriority"函数还考虑 Actor 和 Viewer 之间的相对位置和距离。

![复制变量](../images/image-10.png)

大多数这些设置可以在蓝图的类默认值中找到，也可以在每个 Actor 子级的 C++ 类中进行设置。

``` ini
bOnlyRelevantToOwner = false;
bAlwaysRelevant = false;
bReplicateMovement = true;
bNetLoadOnClient = true;
bNetUseOwnerRelevancy = false;
bReplicates = true;
NetUpdateFrequency = 100.f;
NetCullDistanceSquared = 225000000.f;
NetPriority = 1.f;
```
