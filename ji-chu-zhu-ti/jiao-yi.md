# 交易

交易是由帐户发出，带密码学签名的指令。 帐户将发起交易以更新Hash Ahead网络的状态。 最简单的交易是将 HAH 从一个账户转到另一个帐户。

### 前提条件 <a href="#prerequisites" id="prerequisites"></a>

为了帮助您更好地理解这个页面，我们建议您先阅读[账户](zhang-hu.md)和我们的[Hash Ahead简介](hash-ahead-jian-jie.md)。

### 什么是交易？ <a href="#whats-a-transaction" id="whats-a-transaction"></a>

Hash Ahead交易是指由外部持有账户发起的行动，换句话说，是指由人管理而不是智能合约管理的账户。 例如，如果 Bob 向 Alice 发送 1 HAH，则 Bob 的帐户必须减少 1 HAH，而 Alice 的账户必须增加 1 HAH。 交易会造成状态的改变。

改变 EVMC 状态的交易需要广播到整个网络。 任何节点都可以广播在Hash Ahead虚拟机上执行交易的请求；此后，验证者将执行交易并将由此产生的状态变化传播到网络的其他部分。

交易需要付费并且必须包含在一个有效区块中。 为了使本概述更加简洁，我们将另行介绍燃料费和验证。

所提交的交易包括下列信息：

* `recipient` – 接收地址（如果为一个外部持有的帐户，交易将传输值。 如果为合约帐户，交易将执行合约代码）
* `signature` – 发送者的标识符。 当通过发送者的私钥签名交易来确保发送者已授权此交易时，生成此签名。
* `随机数` - 一个连续的递增计数器，表示帐户中的交易编号。
* `value` – 发送人向接收人转移的 HAH 金额（以 HAH 的一种面值 Wei 为单位）
* `data` – 可包括任意数据的可选字段
* `gasLimit` – 交易可以消耗的最大数量的燃料单位。 燃料单位代表计算步骤
* `maxPriorityFeePerGas` - 作为验证者小费包含的最大燃料数量
* `maxFeePerGas` - 愿意为交易支付的最大燃料数量（包括 `baseFeePerGas` 和 `maxPriorityFeePerGas`）

燃料是指验证者处理交易所需的计算。 用户必须为此计算支付费用。 `gasLimit` 和 `maxPriorityFeePerGas` 决定支付给验证者的最高交易费。 [关于燃料的更多信息](force-fei.md)。

交易对象看起来像这样：

```
{
  from: "0xEA674fdDe714fd979de3EdF0F56AA9716B898ec8",
  to: "0xac03bb73b6a9e108530aff4df5077c2b3d481e5a",
  gasLimit: "21000",
  maxFeePerGas: "300"
  maxPriorityFeePerGas: "10"
  nonce: "0",
  value: "10000000000",
}
```

但交易对象需要使用发送者的私钥签名。 这证明交易只可能来自发送者，而不是欺诈。

示例 [JSON-RPC](../hash-ahead-dui-zhan/hah-ke-hu-duan-api/json-rpc.md) 调用：

```
{
  "id": 2,
  "jsonrpc": "2.0",
  "method": "account_signTransaction",
  "params": [
    {
      "from": "0x1923f626bb8dc025849e00f99c25fe2b2f7fb0db",
      "gas": "0x55555",
      "maxFeePerGas": "0x1234",
      "maxPriorityFeePerGas": "0x1234",
      "input": "0xabcd",
      "nonce": "0x0",
      "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
      "value": "0x1234"
    }
  ]
}
```

示例响应：

```
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "raw": "0xf88380018203339407a565b7ed7d7a678680a4c162885bedbb695fe080a44401a6e4000000000000000000000000000000000000000000000000000000000000001226a0223a7c9bcf5531c99be5ea7082183816eb20cfe0bbc322e97cc5c7f71ab8b20ea02aadee6b34b45bb15bc42d9c09de4a6754e7000908da72d48cc7704971491663",
    "tx": {
      "nonce": "0x0",
      "maxFeePerGas": "0x1234",
      "maxPriorityFeePerGas": "0x1234",
      "gas": "0x55555",
      "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
      "value": "0x1234",
      "input": "0xabcd",
      "v": "0x26",
      "r": "0x223a7c9bcf5531c99be5ea7082183816eb20cfe0bbc322e97cc5c7f71ab8b20e",
      "s": "0x2aadee6b34b45bb15bc42d9c09de4a6754e7000908da72d48cc7704971491663",
      "hash": "0xeba2df809e7a612a0a0d444ccfa5c839624bdc00dd29e3340d46df3870f8a30e"
    }
  }
}
```

* `raw` 是已签名交易的 RLP（Recursive Length Prefix）编码形式。
* `tx` 是已签名交易的 JSON 形式。

如有签名哈希，可通过加密技术证明交易来自发送者并提交网络。

#### `data`字段 <a href="#the-data-field" id="the-data-field"></a>

绝大多数交易都是从外部所有的帐户访问合约。 大多数合约用 Solidity 语言编写，并根据应用程序二进制接口 (ABI) 解释其`data`字段。

前四个字节使用函数名称和参数的哈希指定要调用的函数。 有时可以根据选择器识别函数。

调用数据的其余部分是参数，[按照应用程序二进制接口规范中的规定进行编码](https://docs.soliditylang.org/en/latest/abi-spec.html#formal-specification-of-the-encoding)。

### 交易类型 <a href="#types-of-transactions" id="types-of-transactions"></a>

Hash Ahead有几种不同类型的交易：

* 常规交易：从一个帐户到另一个帐户的交易。
* 合约部署交易：没有“to”地址的交易，数据字段用于合约代码。
* 执行合约：与已部署的智能合约进行交互的交易。 在这种情况下，“to”地址是智能合约地址。

#### 关于燃料 <a href="#on-gas" id="on-gas"></a>

如上所述，执行交易需要耗费[燃料](force-fei.md)。 简单的转账交易需要 21000 单位燃料。

因此，如果 Bob 要在 `baseFeePerGas` 为 190 Gwei 且 `maxPriorityFeePerGas` 为 10 Gwei 时给 Alice 发送一个HAH，Bob 需要支付以下费用：

```
1(190 + 10) * 21000 = 4,200,000 gwei2--或--30.0042 HAH4
```

Bob 的帐户将会扣除 **1.0042 个HAH**（1 个HAH给 Alice，0.0042 个HAH作为燃料费用）

Alice 的帐户将会增加 **+1.0 HAH**

基础费将会燃烧 **-0.00399 HAH**

验证者获得 **0.000210 个HAH**的小费

任何智能合约交互也需要燃料。

任何未用于交易的燃料都会退还给用户帐户。

### 交易生命周期 <a href="#transaction-lifecycle" id="transaction-lifecycle"></a>

交易提交后，就会发生以下情况：

1. 一旦您发送交易，加密法生成交易哈希：&#x20;

```
0x97d99bc7729211111a21b12c933c949d4f31684f1d6954ff477d0477538ff017
```

1. 然后将该交易转播到网络，并且与大量其他交易一起包含在一个集合中。
2. 验证者必须选择你的交易并将它包含在一个区块中，以便验证交易并认为它“成功”。
3. 随着时间的流逝，包含你的交易的区块将升级成“合理”状态，然后变成“最后确定”状态。 通过这些升级，可以进一步确定 你的交易已经成功并将无法更改。 区块一旦“最终确定”，只能通过耗费数十亿美元 的攻击来更改。

\
