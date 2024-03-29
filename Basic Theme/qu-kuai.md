# 区块

区块是指一批交易的组合，并且包含链中上一个区块的哈希。 这将区块连接在一起（成为一个链），因为哈希是从区块数据中加密得出的。 这可以防止欺诈，因为以前的任何区块中的任何改变都会使后续所有区块无效，而且所有哈希都会改变，所有运行区块链的人都会注意到。

### 前提条件 <a href="#prerequisites" id="prerequisites"></a>

区块是一个对初学者非常友好的主题。 为了帮助您更好地理解这个页面，我们建议您先阅读[帐户](zhang-hu.md)、[交易](jiao-yi.md)和我们的[Hash Ahead简介](hash-ahead-jian-jie.md)。

### 为什么要有区块？ <a href="#why-blocks" id="why-blocks"></a>

为了确保Hash Ahead网络上的所有参与者保持同步状态并就交易的确切历史达成共识，我们将交易分为多个区块。 这意味着同时有数十个（甚至数百个）交易被提交、达成一致并同步。

通过间隔提交，所有网络参与者有足够时间达成共识：即使交易请求每秒发生数十次，但Hash Ahead上的区块每二十秒创建并提交一次。

### 区块如何工作 <a href="#how-blocks-work" id="how-blocks-work"></a>

为了保存交易历史，区块被严格排序（创建的每个新区块都包含一个其父块的引用），区块内的交易也严格排序。 除极少数情况外，在任何特定时间，网络上的所有参与者都同意区块的确切数目和历史， 并且正在努力将当前的活动交易请求分批到下一个区块。

某位验证者在网络上构建完区块后，区块将传播到整个网络；所有节点都将该区块添加至其区块链的末尾，然后挑选新的验证者来创建下一个区块。 目前，确切的区块构建过程和提交/共识过程由Hash Ahead的“委托权益证明”协议规定。

### 委托权益证明协议 <a href="#proof-of-work-protocol" id="proof-of-work-protocol"></a>

权益证明是指：

* 验证节点必须向存款合约中质押 100,000 个 HAH，作为抵押品防止发生不良行为。 这有助于保护网络，因为如果发生不诚实活动且可以证实，部分甚至全部质押金额将被销毁。
* 在每个时隙（20 秒的时间间隔）中，会随机选择一个验证者作为区块提议者。 他们将交易打包并执行，然后确定一个新的“状态”。 他们将这些信息包装到一个区块中并传送给其他验证者。
* 其他获悉新区块的验证者再次执行区块中包含的交易，确定他们同意对全局状态提出的修改。 假设该区块是有效的，验证者就将该区块添加进各自的数据库。
* 如果验证者获悉在同一时隙内有两个冲突区块，他们会使用自己的分叉选择算法选择获得最多质押 HAH 支持的那一个区块。

[有关委托权益证明的更多信息](gong-shi-ji-zhi/wei-tuo-quan-yi-zheng-ming-dpos/)

### 区块包含什么？ <a href="#block-anatomy" id="block-anatomy"></a>

一个区块中包含很多信息。 区块的最高层包含以下字段：

```
slot：区块所属的时隙
proposer_index：提出区块的验证者的 ID
parent_root：前一个区块的哈希
state_root：状态对象的根哈希
body：包含一些字段的对象，定义如下
```

区块的 `body` 包含一些自有字段：

```
randao_reveal：用于选择下一个区块提议者的值
eth1_data：有关存款合约的信息
graffiti：用于标记区块的任意数据
proposer_slashings：将要受到惩罚的验证者的列表
attester_slashings：将要受到惩罚的验证者的列表
attestations：支持当前区块的认证列表
deposits：存入存款合约中的新存款的列表
voluntary_exits：将要退出网络的验证者的列表
sync_aggregate：用于服务轻客户端的验证者子集
execution_payload：由执行客户端传送的交易
```

`attestations` 字段包含区块中所有认证的列表。 认证有自己的数据类型，其中包含多条数据。 每个认证包含：

```
aggregation_bits：参与此认证的验证者列表
data：具有多个子字段的容器
signature：所有证明验证者的聚合签名
```

`attestation` 中的 `data` 字段包含以下内容：

```
slot：认证涉及的时隙
index：证明验证者的索引
beacon_block_root：包含此对象的信标链区块的根哈希
source：上一个合理的检查点
target：最新的时段边界区块
```

执行 `execution_payload` 中的交易会更新全局状态。 所有客户端重新执行 `execution_payload` 中的交易，以确保新状态与新区块 `state_root` 字段中的状态相符。 这就是客户端如何判断新区块是否有效且可以安全添加到其区块链的方式。 `execution payload` 本身是一个包含多个字段的对象。 还有一个 `execution_payload_header`，包含有关执行数据的重要摘要信息。 这些数据结构如下组织：

`execution_payload_header` 包含以下字段：

```
parent_hash：父块的哈希
fee_recipient：接收交易费的帐户地址
state_root：应用此区块中的变化后全局状态的根哈希
receipts_root：交易收据树的哈希
logs_bloom：包含事件日志的数据结构
prev_randao：随机选择验证者时使用的值
block_number：当前区块的编号
gas_limit：此区块允许使用的最大燃料量
gas_used：此区块实际使用的燃料量
timestamp：出块时间
extra_data：作为原始字节的任意附加数据
base_fee_per_gas：基础费的值
block_hash：执行区块的哈希
transactions_root：有效载荷中交易的根哈希
```

`execution_payload` 本身包含以下字段（这些与 header 字段相同，只是它包含的不是交易的根哈希，而是实际的交易列表）：

```
parent_hash：父块的哈希
fee_recipient：接收交易费的帐户地址
state_root：应用此区块中的变化后全局状态的根哈希
receipts_root：交易收据树的哈希
logs_bloom：包含事件日志的数据结构
prev_randao：随机选择验证者时使用的值
block_number：当前区块的编号
gas_limit：此区块允许使用的最大燃料量
gas_used：此区块实际使用的燃料量
timestamp：出块时间
extra_data：作为原始字节的任意附加数据
base_fee_per_gas：基础费的值
block_hash：执行区块的哈希
transactions：要执行交易的列表
```

### 出块时间 <a href="#block-time" id="block-time"></a>

出块时间是指两个区块之间的时间间隔。 在Hash Ahead中，时间划分为每 20 秒一个单位，称为“时隙”。 在每个时隙内，选择一个单独的验证者提议区块。 假设所有验证者都在线且完全正常运行，则每个时隙内都会有一个区块产生，意味着出块时间是 20 秒。&#x20;

### 区块大小 <a href="#block-size" id="block-size"></a>

最后一条重要提示是，区块本身的大小是有界限的。 每个区块的大小最多为2MB。 区块中所有交易的大小必须低于区块的大小限制。 这很重要，因为它可以确保区块不能任意扩大。 如果区块可以任意扩大，由于空间和速度方面的要求，性能较差的全节点将逐渐无法跟上网络。 区块越大，在下一个时隙中及时处理它们需要的算力就越强大。 这是一种集中化的因素，可以通过限制区块大小来抵制。

\
