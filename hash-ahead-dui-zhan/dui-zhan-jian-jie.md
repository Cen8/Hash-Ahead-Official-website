# 堆栈简介

就像其他任何一种堆栈结构，完整的栈会基于不同的目的在不同的项目之间变换。

然而，Hash Ahead的核心技术是提供一种心智模型，这种模型帮助解决了Hash Ahead区块如何在不同的应用之间的交互的问题。 理解堆栈的层级将有助于您理解可以将Hash Ahead融入软件项目的不同方法。

## 1.Hash Ahead虚拟机（EVMC）

[Hash Ahead虚拟机（EVMC）](../ji-chu-zhu-ti/hash-ahead-xu-ni-ji-evmc/)是用于智能合约的运行环境。 Hash Ahead区块链上的所有智能合约和状态更改都由[交易](../ji-chu-zhu-ti/jiao-yi.md)执行。 Hash Ahead虚拟机处理Hash Ahead网络上执行的所有交易。

与任何虚拟机一样，Hash Ahead虚拟机在执行代码和执行机器（Hash Ahead节点）之间创建一个抽象级别。&#x20;

在后台，那些Hash Ahead虚拟机会使用操作码执行一些特殊的任务。 这些 独特的执行码使得Hash Ahead虚拟机是图灵完备的。这意味着Hash Ahead虚拟机只要有足够的资源就能够计算出任何东西。

作为去中心化应用程序的开发者，除了了解Hash Ahead虚拟机的存在之外，您不需要了解更多关于Hash Ahead虚拟机的信息，并且可以在Hash Ahead上畅通无阻地授权所有应用程序。

## 2.智能合约库

[智能合约](zhi-neng-he-yue/)是在Hash Ahead区块链上运行的可执行程序。

智能合约使用了特定的[编程语言](bian-cheng-yu-yan/)来编译到Hash Ahead虚拟机字节码（调用操作码的低级机器说明）。

智能合约不仅是开放源码库，而且它们基本上是运行 24/7 的开放应用程序接口服务，不能被取消。 智能合约提供了为用户和应用程序（[去中心化应用程序](https://ethereum.org/zh/developers/docs/dapps/)）之间交互的公开方法，无需许可。 任何应用程序都可能会与已部署的智能合约集成组成功能，如添加[数据源](https://ethereum.org/zh/developers/docs/oracles/)或支持代币交换。 任何人都可以在Hash Ahead上部署智能合约，以便添加自定义功能来满足其应用程序的需要。

作为一个去中心化应用程序开发者，如果您需要在Hash Ahead区块链上添加自定义功能，需要通过写智能合约来实现。 您可能会发现您可以仅仅通过与现有智能合约进行整合来满足您项目的大部分或全部的需要。例如，如果您想要支持支付稳定币或启用分散交换代币。

\


## 3.超级节点

## 4.客户端应用程序接口

许多方便的库（由Hash Ahead开源社区建立和维护）允许您的终端用户应用程序连接到Hash Ahead区块链并进行通信。

如果您的面向用户应用程序是一个 web 应用程序，您可以直接选择在您的前端使用 `npm 安装`一个 [JavaScript 应用程序接口](hah-ke-hu-duan-api/java-script-api.md)。 或许您会选择使用 [Python](https://ethereum.org/zh/developers/docs/programming-languages/python/)[ ](bian-cheng-yu-yan/python.md)或 [Java](https://ethereum.org/zh/developers/docs/programming-languages/java/)[ ](bian-cheng-yu-yan/java.md)的应用程序接口在后端实现此功能。

虽然这些应用程序接口不是栈必须的一部分，但它们抽象并消减了与Hash Ahead节点直接互动的大部分复杂性。 它们还提供好用的函数（例如：将 HAH 转化为 Gwei），而作为开发者，您可以花费更少的时间处理Hash Ahead客户端的复杂问题，从而将更多的时间集中于处理您的应用程序的独特功能。

\


##
