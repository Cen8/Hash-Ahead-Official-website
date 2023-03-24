# 用Web3编写智能合约

## 安装web3、solc依赖包

在开始部署之前，你需要先安装web3、solc的包：

安装Node.js和npm：如果你已经安装了Node.js，可以通过运行npm -v来检查是否已安装npm。如果未安装Node.js，请到官网下载并安装：[https://nodejs.org/en/](https://nodejs.org/en/)

在命令行中输入npm install web3并回车，npm会自动从npm仓库下载并安装web3包。

```
npm install web3
```

安装solc包：在命令行中输入npm install solc并回车，npm会自动从npm仓库下载并安装solc包。

```
npm install solc
```



## 编写智能合约

现在你需要编辑一份智能合约，在此我们将该份智能合约命名为test1.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.8.18;

contract Counter {

    // Public variable of type unsigned int to keep the number of counts
    uint256 public count = 0;

    // Function that increments our counter
    function increment() public {
        count += 1;
    }

    // Not necessary getter to get the count value
    function getCount() public view returns (uint256) {
        return count;
    }

}

```

## 编译部署合约

使用node.js编写代码对test1.sol进行编译以及部署

````javascript
const solc = require('solc');
const Web3 = require('web3');
const fs = require('fs');
//选择区块链节点
const web3 = new Web3('HTTP://127.0.0.1:7545');
// 读取合约文件
const contractCode = fs.readFileSync('./test1.sol', 'utf-8');
//编译合约
const input = {
    language: 'Solidity',
    sources: {
        'test1.sol': {
            content: contractCode,
        },
    },
    settings: {
        outputSelection: {
            '*': {
                '*': ['*']
            },
        },
    },
};
//获取智能合约字节码
const output = JSON.parse(solc.compile(JSON.stringify(input)));
const bytecode = output.contracts['test1.sol']['test'].evm.bytecode.object
//定义智能合约的ABI
const abi = output.contracts['test1.sol']['test'].abi;
//获取一个账户用于智能合约的部署
web3.eth.getAccounts().then(function(accounts) {
    const account = accounts[0];
    //部署智能合约
    const contract = new web3.eth.Contract(abi);
    contract.deploy({
      data: bytecode,
      arguments: []
    }).send({
      from: account,
      gas: 1500000,
      gasPrice: '30000000000000'
    }).then(function(newContractInstance) {
      console.log('Contract deployed at address: ' + newContractInstance.options.address);
    });
  });
```
````

将上述代码保存并命名为test.js，并通过node.js运行，

```
node test.js
```

返回的结果是：

```
Contract deployed at address: 0x9Cc053b6668a36A2C590Ff9b36abE63DaEDFd702
```

至此，该智能合约已成功部署。
