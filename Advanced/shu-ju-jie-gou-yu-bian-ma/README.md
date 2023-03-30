# 数据结构与编码

以太坊会产生、存储和传输大量的数据。 这些数据必须以标准且节约内存的方式格式化，以便任何人都能在相对普通的消费级硬件上[运行节点](../../ji-chu-zhu-ti/jie-dian-yu-ke-hu-duan/yun-hang-jie-dian.md)。 为实现这一目的，以太坊协议栈中使用了一些特殊的数据结构。\


## 数据结构

### 默克尔前缀树 <a href="#patricia-merkle-tries" id="patricia-merkle-tries"></a>

默克尔前缀树是一种数据结构，将给定的键值对编码成具有确定性且经过加密验证的前缀树。 以太坊在其执行层中广泛运用这一数据结构。

[详细了解默克尔前缀树](mo-ke-er-qian-zhui-shu.md)

\


### 递归长度前缀 <a href="#recursive-length-prefix" id="recursive-length-prefix"></a>

递归长度前缀 (RLP) 是一种在以太坊执行层中广泛使用的序列化方法。

[详细了解递归长度前缀](di-gui-chang-du-qian-zhui-bian-ma-rlp.md)

\
