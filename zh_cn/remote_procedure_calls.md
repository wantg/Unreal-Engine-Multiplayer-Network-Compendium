# 远程过程调用

其他复制方式称为"RPC"。"远程过程调用"的缩写形式。

它们用于在另一个实例上调用某些内容。您的电视遥控器对电视也有同样的作用。

虚幻引擎使用它们将事件从客户端发送到服务器、服务器发送到客户端或服务器发送到特定组。

这些 RPC 不能有返回值！要返回某些内容，您需要在另一个方向上使用第二个 RPC。

RPC 仅在特定规则下工作。它们列在下表中，也可以在官方文档中找到：

- 在服务器上运行 - 意味着在此 Actor 的服务器实例上执行
- 在拥有的客户端上运行 - 意味着在此 Actor 的所有者上执行
- NetMulticast - 旨在在此 Actor 的所有实例上执行

## 要求和注意事项​

RPC 要想完全发挥作用，需要满足一些要求：

1. 必须在 Actor 或复制的子对象（例如组件）上调用它们

2. Actor（和组件）必须被复制

3. 如果RPC正在由服务器调用并在客户端上执行，则只有拥有该Actor的客户端才会执行该函数

4. 如果 RPC 由客户端调用并在服务器上执行，则客户端必须拥有调用 RPC 的 Actor

5. 多播 RPC 是一个例外：

    - 如果它们被服务器调用，服务器将在本地执行它们，并在所有当前连接的客户端上执行它们，这些客户端具有相关的 Actor 实例

    - 如果从客户端调用，则组播将仅在本地执行，而不会在服务器或其他客户端上执行

    - 目前，我们对多播事件有一个简单的限制机制：

        - 多播功能在给定 Actor 的网络更新周期内不会复制超过两次。
从长远来看，Epic 预计会对此进行改进。

### 从服务器调用 RPC

| 演员所有权| 未复制 | 网络组播| 服务器| 客户|
| ---------------- | -------------- | ------------ | ------ | ------ |
| 客户拥有的演员 | 在服务器上运行 | 在服务器和所有客户端上运行 | 在服务器上运行 | 在 Actor 拥有的客户端上运行 |
| 服务器拥有的演员| 在服务器上运行 | 在服务器和所有客户端上运行 | 在服务器上运行 | 在服务器上运行 |
| 未婚演员 | 在服务器上运行 | 在服务器和所有客户端上运行 | 在服务器上运行 | 在服务器上运行 |

### 从客户端调用 RPC

| 演员所有权| 未复制 | 网络组播| 服务器| 客户|
| ---------------- | -------------- | ------------ | ------ | ------ |
| 由调用 Client | 拥有 在调用 Client | 时运行 在调用 Client | 时运行 在服务器上运行 | 在调用 Client | 时运行
| 由不同的客户拥有 | 在调用 Client | 时运行 在调用 Client | 时运行 掉落 | 在调用 Client | 时运行
| 服务器拥有的演员| 在调用 Client | 时运行 在调用 Client | 时运行 掉落 | 在调用客户端上运行 |
| 无主演员 | 在调用 Client | 时运行 在调用 Client | 时运行 掉落 | 在调用客户端上运行 |

## 蓝图中的 RPC​

![远程过程调用概述](../images/image-7.png)

蓝图中的 RPC 是通过创建 CustomEvents 并将其设置为 Replicate 来创建的。

![活动详情](../images/image-8.png)

RPC 不能有返回值，因此不能使用函数来创建它们。

"可靠"复选框可用于将 RPC 标记为"重要"，试图确保 RPC 不会被丢弃。

> 注意
>
> 不要将每个 RPC 标记为可靠！您应该只对偶尔调用一次并且需要它们到达目的地的 RPC 执行此操作。
>
> 在 Tick 上调用可靠 RPC 可能会产生副作用，例如填充可靠缓冲区，这可能会导致其他属性和 RPC 不再被处理。

## UE++ 中的 RPC

要在 C++ 中使用整个网络内容，您需要在项目标头中包含"UnrealNetwork.h"。C++ 中的 RPC 相对容易创建，我们只需要在 UFUNCTION() 宏中添加说明符即可。

``` cpp
// 这是一个 ServerRPC，标记为不可靠且 WithValidation（必需！）
UFUNCTION(Server, unreliable, WithValidation)
void Server_Interact();
```

CPP文件将实现不同的功能。这个需要"_Implementation"作为后缀。

``` cpp
// 这是实际的实现，而不是 Server_Interact。但是在调用它时我们使用"Server_Interact"
void ATestPlayerCharacter::Server_Interact_Implementation() {
    // Interact with a door or so!
}
```

CPP 文件还需要一个以"_Validate"作为后缀的版本。稍后会详细介绍这一点。

``` cpp
bool ATestPlayerCharacter::Server_Interact_Validate() {
    return true;
}
```

其他两种类型的 RPC 的创建方式如下：

ClientRPC，需要标记为"可靠"或"不可靠"。

``` cpp
UFUNCTION(Client, unreliable)
void ClientRPCFunction();
```

多播 RPC，也需要标记为"可靠"或"不可靠"。

``` cpp
UFUNCTION(NetMulticast, unreliable)
void MulticastRPCFunction();
```

当然，我们也可以在RPC中添加reliable关键字，使其可靠

``` cpp
UFUNCTION(Client, reliable)
void ReliableClientRPCFunction();

UFUNCTION(NetMulticast, reliable)
void ReliableMulticastRPCFunction();
```

## 验证（UE++）​

验证的想法是，如果 RPC 的验证函数检测到任何参数是错误的，它可以通知系统断开发起 RPC 调用的客户端/服务器。

现在每个 ServerRPCFunction 都需要验证。UFUNCTION 宏中的"WithValidation"关键字就是用于此目的。

``` cpp
UFUNCTION(Server, unreliable, WithValidation)
void SomeRPCFunction(int32 AddHealth);
```

以下是如何使用"_Validate"函数的示例：

``` cpp
bool ATestPlayerCharacter::SomeRPCFunction_Validate(int32 AddHealth) {
    if (AddHealth > MAX_ADD_HEALTH) {
        return false; // This will disconnect the caller!
    }
    return true; // This will allow the RPC to be called!
}
```

> 信息
>
> 客户端到服务器 RPC 需要"_Validate"函数来鼓励安全的服务器 RPC 函数，并使某人尽可能轻松地添加代码来检查每个参数对于所有已知输入约束是否有效。
