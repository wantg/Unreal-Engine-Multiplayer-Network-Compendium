# Actor 角色和 RemoteRole

对于 Actor 复制，我们还有两个更重要的属性。

这两个属性告诉你：

- 谁对 Actor 有权力
- Actor是否被复制
- 复制模式

我们要确定的第一件事是谁对特定 Actor 有权限。

要确定当前运行的引擎实例是否具有权限，请检查角色属性是否为 ROLE_Authority。
如果是，则该引擎实例负责该 Actor（无论是否复制！）。

> 信息
>
> 这与所有权不同！

## 角色/远程角色反转​

Role 和 RemoteRole 可以颠倒，具体取决于谁检查这些值。

例如，如果在服务器上您有以下配置：

- 角色 == Role_Authority

- RemoteRole = ROLE_SimulatedProxy

那么客户看到的会是这样的：

- 角色== ROLE_SimulatedProxy

- 远程角色 == ROLE_Authority

这是有道理的，因为服务器负责 Actor 并将其复制到客户端。
客户端应该接收更新并在更新之间模拟 Actor。

## 复制模式​

服务器不会在每次更新时更新 Actor。这会占用太多的带宽和 CPU 资源。相反，服务器将以"AActor::NetUpdateFrequency"属性指定的频率复制 Actor。

这意味着在 Actor 更新之间客户端会经过一段时间。这可能会导致 Actor 的动作看起来零星或不稳定。为了弥补这一点，客户端将在更新之间模拟 Actor。

目前有两种类型的模拟：

ROLE_SimulatedProxy

和

ROLE_自治代理

### ROLE_SimulatedProxy​

这是标准模拟路径，通常基于根据最后已知速度推断运动。

当服务器发送特定 Actor 的更新时，客户端将向新位置调整其位置，然后在更新之间，客户端将根据服务器发送的最新速度继续移动 Actor。

使用最后已知的速度进行模拟只是一般模拟工作原理的一个示例。

没有什么可以阻止您编写自定义代码来使用其他一些信息来推断服务器更新之间的情况。

### ROLE_AutonomousProxy​

这通常仅用于 PlayerController 拥有的 Actor。

这只是意味着这个 Actor 正在接收来自人类控制器的输入，因此当我们推断时，我们有更多的信息，并且可以使用实际的人类输入来填充缺失的信息（而不是根据最后已知的速度进行推断）。
