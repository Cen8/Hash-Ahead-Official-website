# 用Remix编写智能合约

我猜您和我们一样会很兴奋在HAH区块链上部署智能合约并与之交互。 别担心，作为我们的第一个智能合约，我们会将其部署在本地测试网络上，因此您不需要任何开销就可以随意部署和运行它。

### 第一步 创建智能合约文件

访问 Remix并创建一个新文件。 在 Remix 界面的左上角添加一个新文件，并输入所需的文件名。

<figure><img src="../.gitbook/assets/1.jpg" alt=""><figcaption></figcaption></figure>

### 第二步 编写智能合约代码

在这个新文件中，我们将粘贴如下代码：

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

如果您曾经写过程序，应该可以轻松猜到这个程序是做什么的。 下面按行解释：

第 3 行：定义了一个名为Counter的合约。

第 6 行：我们的合约存储了一个无符号整型count，从 0 开始。

第 9 行：第一个函数将修改合约的状态并且increment()变量 count。

第 14 行，第二个函数是一个 getter 函数，能够从智能合约外部读取count变量的值。 请注意，因为我们将count变量定义为公共变量，所以这个函数是不必要的，但它可以作为一个例子展示。

第一个简单的智能合约到此结束。 正如您所知，它看上去像是 Java、C++这样的面向对象编程语言中的一个类。 现在可以运行我们的合约了。

### 第三步 编译智能合约

当我们写了第一个智能合约后，我们现在可以将它部署在区块链中并运行它。在区块链上部署智能合约实际上只是发送了一个包含已编译智能合约代码的交易，并且没有指定任何收件人。部署合约之前需要先对合约进行编译。

我们首先点击左侧的编译图标来编译合约：

<figure><img src="../.gitbook/assets/2.jpg" alt=""><figcaption></figcaption></figure>

然后点击编译按钮：

<figure><img src="../.gitbook/assets/3.jpg" alt=""><figcaption></figcaption></figure>

您可以选择“Auto compile”选项，这样在将合约内容保存到文本编辑器时合约也随之编译

然后切换到部署和运行交易屏幕：

<figure><img src="../.gitbook/assets/4.jpg" alt=""><figcaption></figcaption></figure>

在“部署和运行交易”屏幕上，仔细检查显示的合约名称并点击“部署”。 正如您在页面顶部所见，当前环境是“JavaScript 虚拟机”，这意味着我们将在本地测试区块链上部署我们的智能合约并与之交互，以便能够更快地进行测试且无须支付任何费用。

点击“部署”按钮后，您可以看到合约在底部显示出来。 点击左侧的箭头展开，可以看到合约的内容。 这里有我们的变量counter、函数increment()和 getter getCounter()。

<figure><img src="../.gitbook/assets/5.jpg" alt=""><figcaption></figcaption></figure>

如果您点击count或getCount按钮，它将实际检索合约的count变量的内容，并显示出来。 因为我们尚未调用increment函数，它应该显示 0。

<figure><img src="../.gitbook/assets/6.jpg" alt=""><figcaption></figcaption></figure>

现在点击按钮来调用increment函数。您可以在窗口底部看到交易产生的日志。当按下检索数据按钮而非increment按钮时，您看到的日志有所不同。 这是因为读取区块链的数据不需要任何交易（写入）或费用。只有修改区块链的状态需要进行交易。

<figure><img src="../.gitbook/assets/7.jpg" alt=""><figcaption></figcaption></figure>

在按下 increment 按钮后，将产生一个交易来调用我们的increment()函数，如果我们点击 count 或 getCount 按钮，将读取我们的智能合约的最新状态，count 变量大于 0

<figure><img src="../.gitbook/assets/8.jpg" alt=""><figcaption></figcaption></figure>
