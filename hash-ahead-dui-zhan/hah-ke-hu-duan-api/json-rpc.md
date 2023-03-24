# JSON-RPC

为了让软件应用程序与Hash Ahead区块链交互（通过读取区块链数据或向网络发送交易），它必须连接到Hash Ahead节点。

为此目的，每个[Hash Ahead客户端](../../ji-chu-zhu-ti/jie-dian-yu-ke-hu-duan/)都实现了一项 [JSON-RPC 规范](https://github.com/ethereum/execution-apis)，因此有一套统一的方法可供应用程序依赖，无论具体的节点或客户端实现如何。

[JSON-RPC](https://www.jsonrpc.org/specification) 是一种无状态的、轻量级远程过程调用 (RPC) 协议。 它定义了一些数据结构及其处理规则。 它与传输无关，因为这些概念可以在同一进程，通过接口、超文本传输协议或许多不同的消息传递环境中使用。 它使用 JSON (RFC 4627) 作为数据格式。

虽然您可以选择通过 JSON 应用程序接口直接与Hash Ahead客户端交互，但是对于去中心化应用程序开发者来说，常常有更容易的选项。 许多 [JavaScript](../bian-cheng-yu-yan/javascript.md) 和[后端应用程序接口](hou-duan-api.md)库已经存在，可以在 JSON-RPC 应用程序接口之上提供封装。 通过这些库，开发者可以方便地写下直观的一行函数来初始化（后端的）JSON RPC 请求并用于与Hash Ahead进行交互。

### JSON-RPC 应用程序接口方法 <a href="#json-rpc-methods" id="json-rpc-methods"></a>

### web3\_clientVersion&#x20;

返回当前客户端版本。

#### 返回值

`String` - 当前客户端版本

#### 示例：

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'
// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result": "Mist/v0.9.3/darwin/go1.4.1"
}e
```

### web3\_sha3

返回给定数据的 Keccak-256（_不是_标准化的 SHA3-256）。

#### 返回值

`DATA` - 给定字符串的 SHA3 结果。

#### 示例：

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":64}'
// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

### net\_version

返回当前网络 id。

#### 返回值

`String` - 当前网络 id。

#### 示例：

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'
// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "3"
}
```

### net\_listening

如果客户端正在主动监听网络连接，则返回 `true`。

#### 返回值

`Boolean` - 监听时为 `true`，否则为 `false`。

#### 示例：

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}'
// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result":true
}
```

### net\_peerCount

返回当前连接到客户端的对等点数。

#### **返回值**

`QUANTITY` - 表示已连接的对等点数的整数。

#### **示例:**

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}'
// Result
{
  "id":74,
  "jsonrpc": "2.0",
  "result": "0x2" // 2
}
```

### eth\_protocolVersion <a href="#eth_protocolversion" id="eth_protocolversion"></a>

返回当前的Hash Ahead协议版本。

#### **返回值**

`String` - 当前的Hash Ahead协议版本

#### **示例:**

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":67}'
// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "54"
}
```

### eth\_syncing <a href="#eth_syncing" id="eth_syncing"></a>

返回一个对象，其中包含有关同步状态的数据或 `false`。

#### **返回值**

`Object|Boolean`，具有同步状态数据的对象，或 `FALSE`（当不同步时）：

* `startingBlock`：`QUANTITY` - 导入开始的区块（只有当同步到达链头后才会重置）
* `currentBlock`：`QUANTITY` - 当前区块，同 eth\_blockNumber
* `highestBlock`：`QUANTITY` - 估计的最高区块

#### **示例**

```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
    startingBlock: '0x384',
    currentBlock: '0x386',
    highestBlock: '0x454'
  }
}
// Or when not syncing
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```

### eth\_coinbase <a href="#eth_coinbase" id="eth_coinbase"></a>

返回客户端 coinbase 地址。

#### **返回值**

`DATA`，20 字节 - 当前的 coinbase 地址。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'
// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
}
```

### eth\_coinbase <a href="#eth_coinbase" id="eth_coinbase"></a>

返回客户端 coinbase 地址。

#### **返回值**

`DATA`，20 字节 - 当前的 coinbase 地址。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'
// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
}
```

### eth\_mining <a href="#eth_mining" id="eth_mining"></a>

如果客户端正在积极挖掘新区块，则返回 `true`。

#### **返回值**

`Boolean` - 如果客户端正在挖矿则返回 `true`，否则返回 `false`。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":71}'
//
{
  "id":71,
  "jsonrpc": "2.0",
  "result": true
}
```

### eth\_hashrate <a href="#eth_hashrate" id="eth_hashrate"></a>

返回节点挖矿时使用的每秒哈希数。

#### **返回值**

`QUANTITY` - 每秒哈希数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_hashrate","params":[],"id":71}'
// Result
{
  "id":71,
  "jsonrpc": "2.0",
  "result": "0x38a"
}
```

### eth\_gasPrice <a href="#eth_gasprice" id="eth_gasprice"></a>

返回单位燃料的当前价格（以 wei 为单位）。

#### **返回值**

`QUANTITY` - 表示当前燃料价格（以 wei 为单位）的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'
// Result
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0x1dfd14000" // 8049999872 Wei
}
```

### eth\_accounts <a href="#eth_accounts" id="eth_accounts"></a>

返回客户端拥有的地址列表。

#### **返回值**

`Array of DATA`，20 字节 - 客户端拥有的地址。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
}
```

### eth\_getBalance <a href="#eth_getbalance" id="eth_getbalance"></a>

返回给定地址的帐户余额。

#### **返回值**

`QUANTITY` - 表示当前余额的整数（以 wei 为单位）。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1", "latest"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

### eth\_blockNumber <a href="#eth_blocknumber" id="eth_blocknumber"></a>

返回最近区块的数量。

#### **返回值**

`QUANTITY` - 表示客户端所在的当前区块号的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":83}'
// Result
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0x4b7" // 1207
}
```

### eth\_getStorageAt <a href="#eth_getstorageat" id="eth_getstorageat"></a>

从给定地址的存储位置返回值。

#### **返回值**

`DATA` - 此存储位置的值。

#### **示例**&#x20;

计算正确位置取决于要检索的存储。 考虑通过地址 `0x391694e7e0b0cce554cb130d723a9d27458f9298` 部署在 `0x295a70b2de5e3953354a6a8344e616ed314d7251` 的以下合约。

```
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;
    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

检索 pos0 的值很简单：

```
curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}' localhost:8545
{"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}
```

### eth\_getTransactionCount <a href="#eth_gettransactioncount" id="eth_gettransactioncount"></a>

返回从一个地址_发送_的交易数量。

#### **返回值**

`QUANTITY` - 表示从该地址发送的交易数量的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1","latest"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

### eth\_getBlockTransactionCountByHash <a href="#eth_getblocktransactioncountbyhash" id="eth_getblocktransactioncountbyhash"></a>

从与给定区块哈希匹配的区块返回区块中的交易数量。

#### **返回值**

`QUANTITY` - 表示此区块中的交易数量的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xb" // 11
}
```

### eth\_getBlockTransactionCountByNumber <a href="#eth_getblocktransactioncountbynumber" id="eth_getblocktransactioncountbynumber"></a>

返回与给定区块号匹配的区块中的交易数量。

#### **返回值**

`QUANTITY` - 表示此区块中的交易数量的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```

### eth\_getUncleCountByBlockHash <a href="#eth_getunclecountbyblockhash" id="eth_getunclecountbyblockhash"></a>

从与给定区块哈希匹配的区块返回区块中的叔块数。

#### **返回值**

`QUANTITY` - 表示此区块中的叔块数量的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleCountByBlockHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

### eth\_getUncleCountByBlockNumber <a href="#eth_getunclecountbyblocknumber" id="eth_getunclecountbyblocknumber"></a>

从与给定区块号匹配的区块返回区块中的叔块数。

#### **返回值**

`QUANTITY` - 表示此区块中的叔块数量的整数。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleCountByBlockNumber","params":["0xe8"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

### eth\_getCode <a href="#eth_getcode" id="eth_getcode"></a>

返回位于给定地址的代码。

#### **返回值**

`DATA` - 来自给定地址的代码。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```

### eth\_sign <a href="#eth_sign" id="eth_sign"></a>

Sign 方法计算Hash Ahead特定的签名：`sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))`。

通过在消息中添加前缀，可以将计算出的签名识别为Hash Ahead特定的签名。 这可以防止恶意去中心化应用程序可以签署任意数据（例如交易）并使用签名冒充受害者的滥用行为。

注意：要签名的地址必须已解锁。

#### **返回值**

`DATA`：签名

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sign","params":["0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", "0xdeadbeaf"],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
}
```

### eth\_signTransaction <a href="#eth_signtransaction" id="eth_signtransaction"></a>

为交易签名，该交易随后可使用 eth\_sendRawTransaction 提交到网络。

**参数**

1. `Object` - 交易对象

* `from`：`DATA`，20 字节 - 发送交易的地址。
* `to`：`DATA`，20 字节 -（创建新合约时可选）将交易定向到的地址。
* `gas`：`QUANTITY` -（可选，默认值：90000）表示为交易执行提供的燃料的整数。 它将返回未使用的燃料。
* `gasPrice`：`QUANTITY` -（可选，默认值：待确定）表示用于每个付费燃料的 gasPrice 的整数，单位为 Wei。
* `value`：`QUANTITY` -（可选）表示与此交易一起发送的价值的整数，单位为 Wei。
* `data`：`DATA` - 合约的编译代码或调用的方法签名和编码参数的哈希。
* `nonce`：`QUANTITY` -（可选）表示随机数的整数。 这允许覆盖你自己的使用相同随机数的待处理交易。

**返回值**

`DATA`，签名的交易对象。

**示例**

```
// Request
curl -X POST --data '{"id": 1,"jsonrpc": "2.0","method": "eth_signTransaction","params": [{"data":"0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675","from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155","gas": "0x76c0","gasPrice": "0x9184e72a000","to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567","value": "0x9184e72a"}]}'
// Result
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
}
```

### eth\_sendTransaction <a href="#eth_sendtransaction" id="eth_sendtransaction"></a>

如果数据字段包含代码，则创建新的消息调用交易或合同创建。

#### **参数**

1. `Object` - 交易对象

* `from`：`DATA`，20 字节 - 发送交易的地址。
* `to`：`DATA`，20 字节 -（创建新合约时可选）将交易定向到的地址。
* `gas`：`QUANTITY` -（可选，默认值：90000）表示为交易执行提供的燃料的整数。 它将返回未使用的燃料。
* `gasPrice`：`QUANTITY` -（可选，默认值：待确定）表示用于每个付费燃料的 gasPrice 的整数。
* `value`：`QUANTITY` -（可选）表示与此交易一起发送的值的整数。
* `data`：`DATA` - 合约的编译代码或调用的方法签名和编码参数的哈希。
* `nonce`：`QUANTITY` -（可选）表示随机数的整数。 这允许覆盖你自己的使用相同随机数的待处理交易。

```
params: [
  {
    from: "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
    to: "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
    gas: "0x76c0", // 30400
    gasPrice: "0x9184e72a000", // 10000000000000
    value: "0x9184e72a", // 2441406250
    data: "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
  },
]
```

#### **返回值**

`DATA`，32 字节 - 交易哈希，如果交易尚不可用，则为零哈希。

当你创建合约时，交易被挖掘后，使用 eth\_getTransactionReceipt 获取合约地址。

**示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{see above}],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

### eth\_sendRawTransaction <a href="#eth_sendrawtransaction" id="eth_sendrawtransaction"></a>

为已签名的交易创建新的消息调用交易或合约创建。

#### **返回值**

`DATA`，32 字节 - 交易哈希，如果交易尚不可用，则为零哈希。

当你创建合约时，交易被挖掘后，使用 [eth\_getTransactionReceipt](https://ethereum.org/zh/developers/docs/apis/json-rpc/#eth\_gettransactionreceipt) 获取合约地址。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{see above}],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

### eth\_call <a href="#eth_call" id="eth_call"></a>

立即执行新的消息调用，而不在区块链上创建交易。

#### **参数**

1. `Object` - 交易调用对象

* `from`：`DATA`，20 字节 -（可选）发送交易的地址。
* `to`：`DATA`，20 字节 - 将交易定向到的地址。
* `gas`：`QUANTITY` -（可选）表示为交易执行提供的燃料的整数。 eth\_call 消耗零燃料，但某些执行可能需要此参数。
* `gasPrice`：`QUANTITY` -（可选）表示用于每个付费燃料的 gasPrice 的整数。
* `value`：`QUANTITY` -（可选）表示与此交易一起发送的值的整数。
* `data`：`DATA` - （可选）方法签名和编码参数的哈希。

2. `QUANTITY|TAG` - 整数区块号，或字符串`“latest”`、`“earliest”`或`“pending”`

#### **返回值**

`DATA` - 已执行合约的返回值。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{see above}],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x"
}
```

### eth\_estimateGas <a href="#eth_estimategas" id="eth_estimategas"></a>

生成并返回允许交易完成所需燃料数量的估算值。 交易不会被添加到区块链中。 请注意，出于各种原因，包括以太坊虚拟机机制和节点性能，估算值可能远远超过交易实际使用的燃料数量。

#### **返回值**

`QUANTITY` - 使用的燃料数量。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{see above}],"id":1}'
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x5208" // 21000
}
```

### eth\_getBlockByHash <a href="#eth_getblockbyhash" id="eth_getblockbyhash"></a>

根据哈希返回关于区块的信息。

#### **返回值**

`Object` - 区块对象，或 `null`（当没有找到区块时）：

* `number`：`QUANTITY` - 区块编号。 当它是待处理区块时，则为 `null`。
* `hash`：`DATA`，32 字节 - 区块的哈希。 当它是待处理区块时，则为 `null`。
* `parentHash`：`DATA`，32 字节 - 父区块的哈希。
* `nonce`：`DATA`，8 字节 - 已生成的工作量证明的哈希。 当它是待处理区块时，则为 `null`。
* `sha3Uncles`：`DATA`，32 字节 - 区块中的叔块数据的 SHA3。
* `logsBloom`：`DATA`，256 字节 - 区块日志的 bloom 过滤器。 当它是待处理区块时，则为 `null`。
* `transactionsRoot`：`DATA`，32 字节 - 区块交易前缀树的根。
* `stateRoot`：`DATA`，32 字节 - 区块最终状态前缀树的根。
* `receiptsRoot`：`DATA`，32 字节 - 区块收据前缀树的根。
* `miner`：`DATA`，20 字节 - 获得挖矿奖励的受益人地址。
* `difficulty`：`QUANTITY` - 表示此区块难度的整数。
* `totalDifficulty`：`QUANTITY` - 表示直到此区块为止链的总难度的整数。
* `extraData`：`DATA` - 此区块的“额外数据”字段。
* `size`：`QUANTITY` - 表示此区块大小的整数，以字节为单位。
* `gasLimit`：`QUANTITY` - 此区块允许的最大燃料值。
* `gasUsed`：`QUANTITY` - 此区块中所有交易使用的总燃料量。
* `timestamp`：`QUANTITY` - 整理区块时的 unix 时间戳。
* `transactions`：`Array` - 交易对象数组，或 32 字节交易哈希，取决于最后一个给定的参数。
* `uncles`：`Array` - 叔块哈希数组。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae", false],"id":1}'
// Result
{
{
"jsonrpc": "2.0",
"id": 1,
"result": {
    "difficulty": "0x4ea3f27bc",
    "extraData": "0x476574682f4c5649562f76312e302e302f6c696e75782f676f312e342e32",
    "gasLimit": "0x1388",
    "gasUsed": "0x0",
    "hash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0xbb7b8287f3f0a933474a79eae42cbca977791171",
    "mixHash": "0x4fffe9ae21f1c9e15207b1f472d5bbdd68c9595d461666602f2be20daf5e7843",
    "nonce": "0x689056015818adbe",
    "number": "0x1b4",
    "parentHash": "0xe99e022112df268087ea7eafaf4790497fd21dbeeb6bd7a1721df161a6657a54",
    "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x220",
    "stateRoot": "0xddc8b0234c2e0cad087c8b389aa7ef01f7d79b2570bccb77ce48648aa61c904d",
    "timestamp": "0x55ba467c",
    "totalDifficulty": "0x78ed983323d",
    "transactions": [
    ],
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "uncles": [
    ]
}
}
```

### eth\_getBlockByNumber <a href="#eth_getblockbynumber" id="eth_getblockbynumber"></a>

根据区块号返回关于区块的信息。

#### **返回值** 参见 [eth\_getBlockByHash](json-rpc.md#eth\_getblockbyhash)

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1b4", true],"id":1}'
```

### eth\_getTransactionByHash <a href="#eth_gettransactionbyhash" id="eth_gettransactionbyhash"></a>

根据交易哈希返回关于所请求交易的信息。

**返回值**

`Object` - 交易对象，如果没有找到交易，则为 `null`：

* `blockHash`：`DATA`，32 字节 - 此交易所在区块的哈希。 当它是待处理区块时，则为 `null`。
* `blockNumber`：`QUANTITY` - 此交易所在的区块号。 当它是待处理区块时，则为 `null`。
* `from`：`DATA`，20 字节 - 发送者的地址。
* `gas`：`QUANTITY` - 发送者提供的燃料。
* `gasPrice`：`QUANTITY` - 发送者提供的燃料价格，以 Wei 为单位。
* `hash`：`DATA`，32 字节 - 交易的哈希。
* `input`：`DATA` - 与交易一起发送的数据。
* `nonce`：`QUANTITY` - 发送者在此交易之前进行的交易数量。
* `to`：`DATA`，20 字节 - 接收者的地址。 当它是合约创建交易时，则为 `null`。
* `transactionIndex`：`QUANTITY` - 表示区块中的交易索引位置的整数。 当它是待处理区块时，则为 `null`。
* `value`：`QUANTITY` - 传输的价值，以 Wei 为单位。
* `v`：`QUANTITY` - ECDSA 恢复 ID
* `r`：`QUANTITY` - ECDSA 签名 r
* `s`：`QUANTITY` - ECDSA 签名 s

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],"id":1}'
// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // 4290000000000000
    "v":"0x25", // 37
    "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
  }
}
```

### eth\_getTransactionByBlockHashAndIndex <a href="#eth_gettransactionbyblockhashandindex" id="eth_gettransactionbyblockhashandindex"></a>

根据区块哈希和交易索引位置返回关于交易的信息。

#### **返回值** 参见 [eth\_getTransactionByHash](json-rpc.md#eth\_gettransactionbyhash)

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'
```

### eth\_getTransactionByBlockNumberAndIndex <a href="#eth_gettransactionbyblocknumberandindex" id="eth_gettransactionbyblocknumberandindex"></a>

根据区块编号和交易索引位置返回关于交易的信息。

#### **返回值** 参见 [eth\_getTransactionByHash](json-rpc.md#eth\_gettransactionbyhash)

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'
```

### eth\_getTransactionReceipt <a href="#eth_gettransactionreceipt" id="eth_gettransactionreceipt"></a>

根据交易哈希返回交易的收据。

**注意**：收据不可用于待处理的交易。

**返回值** `Object` - 交易收据对象，如果没有找到收据，则为 `null`：

* `transactionHash`：`DATA`，32 字节- 交易的哈希。
* `transactionIndex`：`QUANTITY` - 表示区块中的交易索引位置的整数。
* `blockHash`：`DATA`，32 字节 - 此交易所在区块的哈希。
* `blockNumber`：`QUANTITY` - 此交易所在的区块号。
* `from`：`DATA`，20 字节 - 发送者的地址。
* `to`：`DATA`，20 字节 - 接收者的地址。 当它是合约创建交易时，则为 null。
* `cumulativeGasUsed`：`QUANTITY` - 当在区块中执行此交易时使用的燃料总量。
* `gasUsed`：`QUANTITY` - 仅此特定交易使用的燃料数量。
* `contractAddress`：`DATA`，20 字节 - 如果交易是合约创建，则为创建的合约地址，否则为 `null`。
* `logs`：`Array` - 此交易生成的日志对象数组。
* `logsBloom`：`DATA`，256 字符 - 轻量客户端用于快速检索相关日志的 bloom 过滤器。 它还返回_以下两者之一_：
* `root`：`DATA` 32 字节的交易后 stateroot（拜占庭之前）
* `status`：`QUANTITY` - `1`（成功）或 `0`（失败）

**示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'
// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
     logs: [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
     logsBloom: "0x00...0", // 256 byte bloom filter
     status: '0x1'
  }
}
```

### eth\_getUncleByBlockHashAndIndex <a href="#eth_getunclebyblockhashandindex" id="eth_getunclebyblockhashandindex"></a>

根据哈希和叔块索引位置返回关于区块的叔块的信息。

#### **返回值** 参见 [eth\_getBlockByHash](json-rpc.md#eth\_getblockbyhash)

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'
```

### eth\_getUncleByBlockNumberAndIndex <a href="#eth_getunclebyblocknumberandindex" id="eth_getunclebyblocknumberandindex"></a>

根据编号和叔块索引位置返回关于区块的叔块的信息。

#### **返回值** 参见 [eth\_getBlockByHash](json-rpc.md#eth\_getblockbyhash)

**注意**：叔区不包含个人交易。

#### **示例**

```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'
```
