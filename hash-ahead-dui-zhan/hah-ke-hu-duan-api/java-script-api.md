# Java Script API

为了使软件应用程序能够与Hash Ahead区块链进行交互（例如：读取区块链数据或发送交易信息到网络），软件必须连接到Hash Ahead节点。

为此目的，每个Hash Ahead客户端都执行 [JSON-RPC](json-rpc.md) 规范，所以应用程序可以依赖统一的端点集。

如果您想要用 JavaScript 连接到一个Hash Ahead节点， 可以使用原生 JavaScript，不过生态系统中存在一些方便的库，使得这个事情变得更加容易。 通过这些库，开发者可以写下直观易懂甚至单行的代码就能初始化与以太坊的互动（背后使用 JSON RPC 请求）。

运行一个节点需要连接Hash Ahead软件 -- Hash Ahead客户端。  如果你的节点不在本地计算机上（例如，你的节点在 AWS 实例上运行），请相应地更新教程中的 IP 地址。 有关更多信息，请参阅我们关于[运行节点](../../ji-chu-zhu-ti/jie-dian-yu-ke-hu-duan/yun-hang-jie-dian.md)的页面。



## 为什么要使用库？

这些库降低了与一个Hash Ahead节点直接交互的复杂性。 它们还提供实用功能（例如：将HAH转换为 Gwei），因此作为开发者，你可以花费更少的时间处理Hash Ahead客户端的复杂问题，而将更多的时间集中于处理应用程序的独特功能。

## 库功能

#### 连接到Hash Ahead节点 <a href="#connect-to-ethereum-nodes" id="connect-to-ethereum-nodes"></a>

使用提供程序，这些库允许你连接到Hash Ahead并读取它的数据，不管是通过 JSON-RPC还是 Metamask。\


例如：

```javascript
// A Web3Provider wraps a standard Web3 provider, which is
// what MetaMask injects as window.ethereum into each page
const provider = new ethers.providers.Web3Provider(window.ethereum)

// The MetaMask plugin also allows signing transactions to
// send ether and pay to change state within the blockchain.
// 为此，我们需要帐户签名者...
const signer = provider.getSigner()
```

Web3js例子：

```javascript
var web3 = new Web3("http://localhost:9090")
// or
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:9090"))

// change provider
web3.setProvider("ws://localhost:9091")
// or
web3.setProvider(new Web3.providers.WebsocketProvider("ws://localhost:9091"))

// Using the IPC provider in node.js
var net = require("net")
var web3 = new Web3("/Users/myuser/Library/Hashahead/geth.ipc", net) // mac os path
// or
var web3 = new Web3(
  new Web3.providers.IpcProvider("/Users/myuser/Library/Hashahead/geth.ipc", net)
) // mac os path
// on windows the path is: "\\\\.\\pipe\\hah.ipc"
// on linux the path is: "/users/myuser/.hashahead/geth.ipc"

```

一旦设置，你将能够查询区块链的以下内容：

* 区块号
* 燃料估算
* 智能合约事件
* 网络 ID

以及更多...

## 钱包功能

这些库为你提供了创建钱包、管理密匙和签署交易的功能。

这里提供了 Ethers 中的一个示例：

```javascript
// 从助记符创建一个钱包实例...
mnemonic =
  "announce room limb pattern dry unit scale effort smooth jazz weasel alcohol"
walletMnemonic = Wallet.fromMnemonic(mnemonic)

// ...或者从一个私有密匙中创建
walletPrivateKey = new Wallet(walletMnemonic.privateKey)

walletMnemonic.address === walletPrivateKey.address
// true

// 每个签署 API 都是一个异步操作地址
walletMnemonic.getAddress()
// { Promise: '0x71CB05EE1b1F506fF321Da3dac38f25c0c9ce6E1' }

// 每个钱包地址同时也可以是同步的
walletMnemonic.address
// '0x71CB05EE1b1F506fF321Da3dac38f25c0c9ce6E1'

// 内部加密组件
walletMnemonic.privateKey
// '0x1da6847600b0ee25e9ad9a52abbd786dd2502fa4005dd5af9310b7cc7a3b25db'
walletMnemonic.publicKey
// '0x04b9e72dfd423bcf95b3801ac93f4392be5ff22143f9980eb78b3a860c4843bfd04829ae61cdba4b3b1978ac5fc64f5cc2f4350e35a108a9c9a92a81200a60cd64'

// 钱包的助记符（mnemonic）
walletMnemonic.mnemonic
// {
//   locale: 'en',
//   path: 'm/44\'/60\'/0\'/0/0',
//   phrase: 'announce room limb pattern dry unit scale effort smooth jazz weasel alcohol'
// }

// 注意：使用私有密匙创建的钱包没有
//       从助记符（被原生屏蔽）
walletPrivateKey.mnemonic
// null

// 签署一个信息
walletMnemonic.signMessage("Hello World")
// { Promise: '0x14280e5885a19f60e536de50097e96e3738c7acae4e9e62d67272d794b8127d31c03d9cd59781d4ee31fb4e1b893bd9b020ec67dfa65cfb51e2bdadbb1de26d91c' }

tx = {
  to: "0x8ba1f109551bD432803012645Ac136ddd64DBA72",
  value: utils.parseEther("1.0"),
}

// 签署一笔交易
walletMnemonic.signTransaction(tx)
// { Promise: '0xf865808080948ba1f109551bd432803012645ac136ddd64dba72880de0b6b3a7640000801ca0918e294306d177ab7bd664f5e141436563854ebe0a3e523b9690b4922bbb52b8a01181612cec9c431c4257a79b8c9f0c980a2c49bb5a0e6ac52949163eeb565dfc' }

// 这个连接方法会返回一个新的连接到提供者的钱包实例
wallet = walletMnemonic.connect(provider)

// 查询以太坊网络
wallet.getBalance()
// { Promise: { BigNumber: "42" } }
wallet.getTransactionCount()
// { Promise: 0 }

// 发送 Ether
wallet.sendTransaction(tx)

```

一旦设置，你将能够：

* 创建帐户
* 发送交易
* 签署交易

以及更多...

## 与智能合约交互的方法 <a href="#interact-with-smart-contract-functions" id="interact-with-smart-contract-functions"></a>

JavaScript 客户端库允许你的应用程序通过读取已编译合约的应用程序二进制接口 (ABI) 来调用智能合约函数。

ABI 本质上是以 JSON 格式解释了合约的功能，并且允许你像普通 JavaScript 对象一样使用它。

以下 Solidity 合约：\


```solidity
contract Test {
    uint a;
    address d = 0x12345678901234567890123456789012;

    function Test(uint testInt)  { a = testInt;}

    event Event(uint indexed b, bytes32 c);

    event Event2(uint indexed b, bytes32 c);

    function foo(uint b, bytes32 c) returns(address) {
        Event(b, c);
        return d;
    }
}
```

将会产生以下 JSON 代码：

```json
[{
    "type":"constructor",
    "payable":false,
    "stateMutability":"nonpayable"
    "inputs":[{"name":"testInt","type":"uint256"}],
  },{
    "type":"function",
    "name":"foo",
    "constant":false,
    "payable":false,
    "stateMutability":"nonpayable",
    "inputs":[{"name":"b","type":"uint256"}, {"name":"c","type":"bytes32"}],
    "outputs":[{"name":"","type":"address"}]
  },{
    "type":"event",
    "name":"Event",
    "inputs":[{"indexed":true,"name":"b","type":"uint256"}, {"indexed":false,"name":"c","type":"bytes32"}],
    "anonymous":false
  },{
    "type":"event",
    "name":"Event2",
    "inputs":[{"indexed":true,"name":"b","type":"uint256"},{"indexed":false,"name":"c","type":"bytes32"}],
    "anonymous":false
}]

```

这意味着你可以：

* 发送一笔交易到指定的智能合约上，并执行智能合约上的方法
* 调用方法去评估对气体的需求量。这个方法的执行是在Hash Ahead虚拟机中执行的。
* 部署一个合约

以及更多...
