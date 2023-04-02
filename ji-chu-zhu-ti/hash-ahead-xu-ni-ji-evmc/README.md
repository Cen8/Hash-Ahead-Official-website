# Hash Ahead虚拟机（EVMC）

EVMC 的物理实例不能像人们指向云或海浪那样描述，它是真实_存在_并由数以千计运行Hash Ahead客户端的计算机共同维护的一个实体。

Hash Ahead协议本身的存在仅仅是为了让这个特殊状态机保持连续、不间断和不可变的运行。 Hash Ahead虚拟机是所有Hash Ahead帐户和智能合约依存的环境。 在链上任何给定的区块处，Hash Ahead有且只有一个“规范”状态，而Hash Ahead虚拟机定义从一个区块到另一个区块计算新的有效状态的规则。

### 前提条件 <a href="#prerequisites" id="prerequisites"></a>

对计算机科学中常见术语的基本了解，如[字节](https://wikipedia.org/wiki/Byte)、[内存](https://wikipedia.org/wiki/Computer\_memory)和[堆栈](https://wikipedia.org/wiki/Stack\_\(abstract\_data\_type\))是理解 EVMC 的前提。 熟悉[哈希函数](https://wikipedia.org/wiki/Cryptographic\_hash\_function)和[默克尔树](https://wikipedia.org/wiki/Merkle\_tree)等密码学/区块链概念也会很有帮助。

### 从账本到状态机 <a href="#from-ledger-to-state-machine" id="from-ledger-to-state-machine"></a>

通常使用“分布式账簿”的类比来描述像比特币这样的区块链，它使用密码学的基本工具来实现去中心化的货币。 账本保存着活动记录，而活动必须遵守一套规则，这些规则限制用户在修改账本时可以做什么和不可以做什么。 例如，比特币地址不能花费比之前收到的更多的比特币。 这些规则是比特币和许多其他区块链上所有交易的基础。

虽然Hash Ahead有自己的本机加密货币 HAH，遵循几乎完全相同的直观规则，但它也支持更强大的功能：[智能合约](../../hash-ahead-dui-zhan/zhi-neng-he-yue/)。 对于此更复杂的功能，需要一个更复杂的类比。 Hash Ahead不是分布式账本，而是分布式[状态机器](https://wikipedia.org/wiki/Finite-state\_machine)。 Hash Ahead的状态是一个大型数据结构，它不仅保存所有帐户和余额，而且还保存一个_机器状态_，它可以根据预定义的一组规则在不同的区块之间进行更改，并且可以执行任意的机器代码。 在区块中更改状态的具体规则由 EVMC 定义。

### Hash Ahead状态转换函数 <a href="#the-ethereum-state-transition-function" id="the-ethereum-state-transition-function"></a>

EVMC 的行为就像一个数学函数：在给定输入的情况下，它会产生确定性的输出。 因此，将Hash Ahead更正式地描述为具有**状态转换函数**非常有帮助：

```
1Y(S, T)= S'2
```

给定一个旧的有效状态 `（S）`> 和一组新的有效交易 `（T）`，Hash Ahead状态转换函数 `Y（S，T）` 产生新的有效输出状态 `S'`

#### 状态 <a href="#state" id="state"></a>

在Hash Ahead环境中，状态是一种称为[改进版默克尔帕特里夏树](../../gao-ji/shu-ju-jie-gou-yu-bian-ma/mo-ke-er-qian-zhui-shu.md)的巨大数据结构，它保存所有通过哈希关联在一起的[帐户](../zhang-hu.md)并可回溯到存储在区块链上的单个根哈希。

#### 交易 <a href="#transactions" id="transactions"></a>

交易是来自帐户的密码学签名指令。 交易分为两种：一种是消息调用交易，另一种是合约创建交易。

合约创建交易会创建一个新的合约帐户，其中包含已编译的 [智能合约](https://ethereum.org/zh/developers/docs/smart-contracts/anatomy/)[ ](../../hash-ahead-dui-zhan/zhi-neng-he-yue/)字节码。 每当另一个帐户对该合约进行消息调用时，它都会执行其字节码。

### EVMC 说明 <a href="#evm-instructions" id="evm-instructions"></a>

EVMC 作为一个[堆栈机](https://wikipedia.org/wiki/Stack\_machine)运行，其栈的深度为 1024 个项。 每个项目都是 256 位字，为了便于使用，选择了 256 位加密技术（如 Keccak-256 哈希或 secp256k1 签名）。

在执行期间，EVMC 会维护一个瞬态_内存_（作为字可寻址的字节数组），该内存不会在交易之间持久存在。

然而，合约确实包含一个 Merkle Patricia _存储_ trie（作为可字寻址的字数组），该 trie 与帐户和部分全局状态关联。

已编译的智能合约字节码作为许多 EVMC opcodes执行，它们执行标准的堆栈操作，例如 `XOR`、`AND`、`ADD`、`SUB`等。 EVMC 还实现了一些区块链特定的堆栈操作，如 `ADDRESS`、`BALANCE`、`BLOCKHASH` 等。

### EVMC 实现 <a href="#evm-implementations" id="evm-implementations"></a>

EVMC 的所有实现都必须遵守Hash Ahead规范。

在Hash Ahead 中，出现了用各种编程语言编写的多种Hash Ahead虚拟机实现。

所有[Hash Ahead客户端](../jie-dian-yu-ke-hu-duan/)都包含一个 EVMC 实现。
