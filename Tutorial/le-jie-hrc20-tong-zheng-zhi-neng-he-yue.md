# 了解HRC-20通证智能合约

[HRC-20](https://app.gitbook.com/s/495EEItG0CJPjZDJD1T3/gao-ji/biao-zhun-hip/token-biao-zhun/hrc20-tong-zhi-hua-dai-bi)是Hash Ahead上最重要的[智能合约标准](https://ethereum.org/zh/developers/docs/standards/)之一。它已经成为Hash Ahead区块链上用于可替换通证实现的所有智能合约的技术标准。

HRC-20 定义了所有可替换的Hash Ahead通证都应该遵守的通用规则列表。 因此，该通证标准使所有类型的开发者能够准确预测新通证在更大的Hash Ahead系统中将如何工作。 这简化了开发者的任务，因为他们可以继续他们的工作，知道只要通证遵循规则，每次发布新的通证时就不需要重做每个新项目。

这里以接口的形式介绍了 HRC-20 必须实现的函数。

```solidity
pragma solidity ^0.8.18;

interface IHRC20 {

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);


    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

```

下面逐行解释每个函数的用途。 在这之后，我们将展示一个 HRC-20 通证的简单实现。

## 取值器 <a href="#getters" id="getters"></a>

```
function totalSupply() external view returns (uint256);
```

返回存在的通证数量。 此函数是一个取值器，不会修改合约的状态。 请记住，Solidity 中没有浮点数。 因此，大多数通证都会采用 18 位小数，并且会返回总供应量和其他结果，如下所示：1 个通证 100000000000000000。 您需要在处理通证时格外注意，并不是每个通证都有 18 位小数。

```
function balanceOf(address account) external view returns (uint256);
```

HRC-20 标准使一个地址能够允许另一个地址从中检索通证。 此取值器返回允许`spender`代表`owner`花费的剩余通证数量。 此函数是一个取值器，不会修改合约的状态，并且默认应返回 0。

## 函数 <a href="#functions" id="functions"></a>

```
function transfer(address recipient, uint256 amount) external returns (bool);
```

将通证的`amount`从函数调用者地址(`msg.sender`) 移动到接收者地址。 此函数发出稍后定义的`Transfer`事件。 如果可进行转账，它将返回 true。

```
function approve(address spender, uint256 amount) external returns (bool);
```

设置允许`spender`从函数调用方(`msg.sender`)余额转账的`allowance`的数额。 此函数发出 Approval 事件。 此函数返回是否成功设置了余量。

```
function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
```

使用余量机制将通证的`amount`从`sender`移动到`recipient`。 然后从调用者的余量中扣除该数额。 此函数发出`Transfer`事件。

## 事件

```
event Transfer(address indexed from, address indexed to, uint256 value);
```

将通证（值）的数量从`from`地址发送到`to`地址时会发出此事件。

在铸造新代币时，转账通常会在 `from` 0x00..0000 地址进行，而在销毁代币时，转账会在 `to` 0x00..0000 地址进行。

```
event Approval(address indexed owner, address indexed spender, uint256 value);
```

当`owner`批准要由`spender`使用的通证数量(`value`)时，将发出此事件。

ERC-20 通证的基本实现\
 <a href="#a-basic-implementation-of-erc-20-tokens" id="a-basic-implementation-of-erc-20-tokens"></a>
-----------------------------------------------------------------------------------------------------

下面是 ERC-20 通证的最简单代码：

```solidity
pragma solidity ^0.8.18;

interface IHRC20 {

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);


    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


contract HRC20Basic is IHRC20 {

    string public constant name = "HRC20Basic";
    string public constant symbol = "HRC";
    uint8 public constant decimals = 18;


    mapping(address => uint256) balances;

    mapping(address => mapping (address => uint256)) allowed;

    uint256 totalSupply_ = 10 ether;


   constructor() {
    balances[msg.sender] = totalSupply_;
    }

    function totalSupply() public override view returns (uint256) {
    return totalSupply_;
    }

    function balanceOf(address tokenOwner) public override view returns (uint256) {
        return balances[tokenOwner];
    }

    function transfer(address receiver, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[msg.sender]);
        balances[msg.sender] = balances[msg.sender]-numTokens;
        balances[receiver] = balances[receiver]+numTokens;
        emit Transfer(msg.sender, receiver, numTokens);
        return true;
    }

    function approve(address delegate, uint256 numTokens) public override returns (bool) {
        allowed[msg.sender][delegate] = numTokens;
        emit Approval(msg.sender, delegate, numTokens);
        return true;
    }

    function allowance(address owner, address delegate) public override view returns (uint) {
        return allowed[owner][delegate];
    }

    function transferFrom(address owner, address buyer, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[owner]);
        require(numTokens <= allowed[owner][msg.sender]);

        balances[owner] = balances[owner]-numTokens;
        allowed[owner][msg.sender] = allowed[owner][msg.sender]-numTokens;
        balances[buyer] = balances[buyer]+numTokens;
        emit Transfer(owner, buyer, numTokens);
        return true;
    }
}

```
