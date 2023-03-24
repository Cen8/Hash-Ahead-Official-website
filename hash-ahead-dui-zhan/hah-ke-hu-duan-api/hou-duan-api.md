# 后端API

为了使软件应用程序能够与Hash Ahead区块链进行交互（例如：读取区块链数据或发送交易信息到网络），软件必须连接到Hash Ahead节点。

为此目的，每个Hash Ahead客户端都执行 [JSON-RPC](https://ethereum.org/zh/developers/docs/apis/json-rpc/) 规范，所以应用程序可以依赖统一的[端点](https://ethereum.org/zh/developers/docs/apis/json-rpc/#json-rpc-methods)集。

如果您想使用特定的编程语言去连接Hash Ahead的节点，您可自行选择，但是在社区中已有几个方便的库，可以更方便地实现应用程序与Hash Ahead的连接。 通过这些库，开发者可以方便地写下直观的一行函数来初始化（后端的）JSON RPC 请求并用于与Hash Ahead进行交互。



## 为什么要使用库？

这些库降低了与一个Hash Ahead节点交互的复杂性。 它们还提供实用的函数（例如：将 HAH 转化为 Gwei），而作为开发者，您可以花费更少的时间来处理Hash Ahead客户端的复杂问题，从而将更多的时间集中于处理您的应用程序的独特功能。

\
\
