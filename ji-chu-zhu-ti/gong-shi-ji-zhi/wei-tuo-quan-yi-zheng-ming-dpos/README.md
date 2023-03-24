# 委托权益证明（DPoS）

委托权益证明 (DPoS) 是支撑Hash Ahead[共识机制](https://ethereum.org/zh/developers/docs/consensus-mechanisms/)的基础。这是因为相比于[工作量证明](https://ethereum.org/zh/developers/docs/consensus-mechanisms/pow/)（POW）和权益证明（POS）的架构相比，Hash Ahead更安全、消耗更低，并且更利于实现新的扩容解决方案。

## 什么是委托权益证明？

DPoS是Delegated Proof of Stake的缩写，是一种基于权益证明的共识算法。DPoS的基本思想是将权益（代币）的持有者委托给一些代表节点，由代表节点共同维护整个网络的运行和安全。这些代表节点会根据自身的能力和贡献竞选出来，被选中的代表节点可以获得一定的奖励。

相比于PoW（Proof of Work）算法，DPoS算法的优点在于：

* 能够避免算力浪费。在PoW算法中，大量的算力被浪费在了“竞争出块”的过程中，而在DPoS算法中，节点的选举和出块是由持币者投票决定的，因此算力被充分利用，不会浪费。
* 可以提高交易速度。由于DPoS算法中代表节点的数量相对较少，因此出块速度可以更快，网络的交易速度也可以更快。
* 能够提高网络的去中心化程度。在DPoS算法中，代表节点可以由持币者自由选择和投票，因此网络的控制权可以更加分散，去中心化程度也可以更高。

\
