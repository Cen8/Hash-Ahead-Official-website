# 面向 PYTHON 开发者的Hash Ahead介绍

### 前提条件 <a href="#soft-prerequisites" id="soft-prerequisites"></a>

本文希望面向所有开发者。 在文章里会涉及 [Python 工具](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/hash-ahead-dui-zhan/bian-cheng-yu-yan/python)，不过它们只是思想的载体，如果您不是 Python 开发者也没有问题。 不过，我将对您已经了解的知识作一些假设，以便我们能够迅速地进入Hash Ahead部分。

本文假定：

* 您熟悉终端操作，
* 您写过一些 Python 代码，
* 您的机器上有安装 Python 3.6 或更高版本 (强烈推荐使用 [虚拟环境](https://realpython.com/effective-python-environment/#virtual-environments) )，并且
* 您使用过 `pip`，Python 的软件包安装程序。 再次强调，如果您不符合其中任何一条，或者您不打算敲本文中的代码，您照着学仍然可以学得很好。

### 区块链简述 <a href="#blockchains-briefly" id="blockchains-briefly"></a>

描述Hash Ahead有很多方法，但其核心还是区块链。 区块链由一系列区块组成，所以让我们从区块链开始。 用最简单的话来说，Hash Ahead区块链上的每个区块只是一些元数据和一个交易的列表。 在 JSON 格式中，它看起来像这样：

```
{
   "number": 1234567,
   "hash": "0xabc123...",
   "parentHash": "0xdef456...",
   ...,
   "transactions": [...]
}
```

每个[ 块 ](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/ji-chu-zhu-ti/qu-kuai)会引用它前面的区块； `parentHash` 是前一个区块的哈希值。

这种数据结构并不新颖，但治理网络的规则（即点对点协议）却很新颖。 区块链没有中央机构；网络中的对等节点必需协作以维持网络，并且通过竞争决定将哪些交易纳入下一个区块。 因此，当您想给朋友转账时，您需要将这笔交易广播到网络上，然后等待它被纳入即将产生的区块。

区块链验证资金确实从一个用户发送给另一个用户的唯一方法是使用该区块链原生货币（即，由该区块链创建和管理的货币）。 在Hash Ahead，这种货币被称为 HAh，Hash Ahead区块链是账户余额的唯一正式记录。

## 一种新范式 <a href="#a-new-paradigm" id="a-new-paradigm"></a>

这种新的去中心化技术栈催生了新的开发者工具。 许多编程语言都有这样的工具，但我们将通过 Python 的视角来观察。 重申一下：即使 Python 不是您的首选语言，跟上文章也不会有什么太大的问题。

想要与Hash Ahead进行互动的 Python 开发人员可能会接触到 [Web3.py](https://web3py.readthedocs.io/)。 Web3.py 是一个库，可以帮助我们简化连接Hash Ahead节点，以及发送和接收数据。

[Hash Ahead客户端](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/ji-chu-zhu-ti/jie-dian-yu-ke-hu-duan)可以配置为通过[进程间通信 (IPC)](https://wikipedia.org/wiki/Inter-process\_communication)、超文本传输协议 (HTTP) 或网络套接字 (Websockets) 进行访问，因此 Web3.py 也需要完成这个配置。 Web3.py 将这些连接选项称为**提供者**。 您需要从三个提供者中选择一个来连接 Web3.py 实例和您的节点。

正确配置了 Web3.py 之后，您就可以开始与区块链交互了。 下面是一些 Web3.py 使用示例，算是抛砖引玉：

```
# read block data:
w3.eth.get_block('latest')

# send a transaction:
w3.eth.send_transaction({'from': ..., 'to': ..., 'value': ...})
```

## 安装 <a href="#installation" id="installation"></a>

首先，安装 [IPython](https://ipython.org/)，以方便用户在其中进行探索。 IPython 提供了 tab 补全等功能，使得我们更容易看到 Web3.py 中有哪些可用方法。

```
$ pip install ipython
```

Web3.py 以 `web3` 的名称发布。 安装方式如下：

```
$ pip install web3
```

另外，我们后面要模拟一个区块链，这就需要更多依赖项。 可以通过下面的命令安装这些依赖项：

```
$ pip install 'web3[tester]'
```

在终端中运行 `ipython`，打开一个新的 Python 环境。 这与运行 `python` 类似，但更友好。

```
$ ipython
```

这将打印出一些关于您正在运行的 Python 和 IPython 版本的信息，然后您应该会看到一个等待输入的提示：

```
In [1]:2
```

您现在看到的是一个交互式的 Python shell， 实际上，它是一个沙盒。 如果您已经做到了这一点，现在可以导入 Web3.py 了：

```
In [1]: from web3 import Web3
```

## WEB3 模块介绍 <a href="#introducing-the-web3-module" id="introducing-the-web3-module"></a>

除了作为Hash Ahead的网关， [Web3](https://web3py.readthedocs.io/en/stable/overview.html#base-api)模块还提供了一些便利的功能。 让我们来探究探究。

在Hash Ahead应用中，您通常需要转换货币面额。

试一下将一些数值转换为 wei 或反向转换。 请注意， HAH 和 wei 之间还有其他面额名称。 其中比较有名的是 **gwei**，因为它通常用于表示交易费用。

```
In [2]: Web3.toWei(1, 'ether')
Out[2]: 1000000000000000000

In [3]: Web3.fromWei(500000000, 'gwei')
Out[3]: Decimal('0.5')

```

Web3 模块上的其他实用方法包括数据格式转换器（例如 [`toHex`](https://web3py.readthedocs.io/en/stable/web3.main.html#web3.Web3.toHex)），地址助手，（例如 [`is address`](https://web3py.readthedocs.io/en/stable/web3.main.html#web3.Web3.isAddress)），以及哈希函数（例如 [`keccak`](https://web3py.readthedocs.io/en/stable/web3.main.html#web3.Web3.keccak)）。 其中许多内容将在后面的系列文章中介绍。 要查看所有可用的方法和属性，可以利用 IPython 的自动补全功能，输入 `Web3`。 然后在点号后面按两次 tab 键。

## 快速了解

第一件事，先进行连接检查。

```
In [5]: w3.isConnected()
Out[5]: True

```

由于我们使用的是测试器提供者，这不是一个非常有价值的测试，但如果它确实失败了，很可能是在实例化 `w3` 变量时发生了输入错误。 仔细检查您是否包含了内括号，即 `Web3.EthereumTesterProvider()`。

### 第一站：[帐户](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/ji-chu-zhu-ti/zhang-hu) <a href="#tour-stop-1-accounts" id="tour-stop-1-accounts"></a>

为了方便起见，测试器提供者创建了一些帐户，并预先分配了测试HAH。

首先，我们来看看这些帐户的列表：

```
In [6]: w3.eth.accounts
Out[6]: ['0x7E5F4552091A69125d5DfCb7b8C2659029395Bdf',
 '0x2B5AD5c4795c026514f8317c7a215E218DcCD6cF',
 '0x6813Eb9362372EEF6200f3b1dbC3f819671cBA69', ...]

```

如果运行这个命令，您应该会看到一个以 `0x` 开头的十个字符串的列表。 每一个字符串都是一个的**公共地址**，在某些方面，类似于支票帐户上的帐号。 如果有人要给您转 HAH，您可以把这个地址给他。

如前所述，测试提供者已经为这些账户中的每一个账户预分配了一些测试HAH。 我们来看看第一个帐户上有多少 HAH。

```
In [7]: w3.eth.get_balance(w3.eth.accounts[0])
Out[7]: 1000000000000000000000000

```

将其转换为 HAH：

```
In [8]: w3.fromWei(1000000000000000000000000, 'ether')
Out[8]: Decimal('1000000')
```

总共100 万测试HAH

### 第二站：区块数据 <a href="#tour-stop-2-block-data" id="tour-stop-2-block-data"></a>

我们来看看这个模拟区块链的状态。

```
In [9]: w3.eth.get_block('latest')
Out[9]: AttributeDict({
   'number': 0,
   'hash': HexBytes('0x9469878...'),
   'parentHash': HexBytes('0x0000000...'),
   ...
   'transactions': []
})

```

这里返回了大量关于区块的信息，但这里只介绍以下几点：

* `transactions` 是一个空列表，原因相同：我们还没有做任何事情。 第一个区块是一个**空区块**，只是为了开个头。
* 注意，`parentHash` 只是一堆空的字节。 这标志着它是链条上的第一个区块，也就是所谓的**创世区块**。

### 第三站： [交易](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/ji-chu-zhu-ti/jiao-yi) <a href="#tour-stop-3-transactions" id="tour-stop-3-transactions"></a>

在没有待处理交易之前，我们停留在零区块处，所以我们给它一个交易。 从一个账户向另一个账户发送一些测试 HAH：

```
In [10]: tx_hash = w3.eth.send_transaction({
   'from': w3.eth.accounts[0],
   'to': w3.eth.accounts[1],
   'value': w3.toWei(3, 'ether'),
   'gas': 21000
})

```

这时你通常会等上几秒钟，等待交易添加到新区块中。 完整的流程是这样的：

1. 提交交易并持有交易哈希。 在包含交易的区块被创建并广播之前，交易一直处于“待处理”状态。 `tx_hash = w3.eth.send_transaction({ … })`
2. 等待交易添加到区块中： `w3.eth.wait_for_transaction_receipt(tx_hash)`
3. 继续应用逻辑。 查看成功的交易：`w3.eth.get_transaction(tx_hash)`

我们的模拟环境会在一个新的区块中即时添加交易，所以我们可以立即查看交易：

```
In [11]: w3.eth.get_transaction(tx_hash)
Out[11]: AttributeDict({
   'hash': HexBytes('0x15e9fb95dc39...'),
   'blockNumber': 1,
   'transactionIndex': 0,
   'from': '0x7E5F4552091A69125d5DfCb7b8C2659029395Bdf',
   'to': '0x2B5AD5c4795c026514f8317c7a215E218DcCD6cF',
   'value': 3000000000000000000,
   ...
})

```

您将在这里看到一些熟悉的细节：`from`、`to` 和 `value` 字段应该与 `send_transaction` 调用的输入相匹配。 另一个令人欣慰的是，这项交易被列为 1 号区块内的第一笔交易（`'transactionIndex': 0`）。

我们也可以通过检查两个相关帐户的余额，轻松验证此次交易是否成功。 三个 HAH 应从一个帐户转移到另一个帐户。

```
In [12]: w3.eth.get_balance(w3.eth.accounts[0])
Out[12]: 999996999999999999969000

In [13]: w3.eth.get_balance(w3.eth.accounts[1])
Out[13]: 1000003000000000000000000

```

余额从 1000000 增加到 1000003 个 HAH。但第一个账户由于使用Hash Ahead网络需要支付矿工手续费， 一笔小额交易费从进行交易的帐户中扣除，金额为 31000 wei。\
