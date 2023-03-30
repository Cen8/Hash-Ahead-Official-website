# 客户端安装

### 本地或云端

Hash Ahead客户端能够在消费级电脑上运行，并且不需要任何专用硬件，例如矿机。 因此，你可以根据需要选择多种节点部署方案。 简而言之，我们考虑在本地物理计算机和云端服务器上运行节点：

* 云端
  * 服务商提供了高可用的服务器以及静态公共 IP 地址
  * 获得专用或虚拟服务器比自己搭建更加方便
  * 取舍是：是否需要信赖云服务商三方
  * 由于全节点所需的存储大小，租用服务器的价格可能会很高
* 自有硬件
  * 更可信并且更有主动权
  * 只需一次性投入
  * 可以购买预先配置好的机器
  * 你必须亲自准备并维护机器和网络，并有可能亲自对机器和网络进行故障排除

### 配置环境

您可以在运行 64 位 Linux 的计算机上安装和设置开发环境。

在 Linux 设备上安装和设置开发环境之前，请确保您的计算机满足以下基本要求：

#### **最低要求 {#minimum-requirements}**

* 2 核以上 CPU
* 4 GB 内存
* 500GB 可用硬盘空间
* 10 MB/秒以上带宽

&#x20;

#### **推荐的规格要求 {#recommended-hardware}**

* 4 核以上快速 CPU
* 8GB 以上内存
* 1TB 以上高速固态硬盘
* 20 MB/秒以上带宽

### 安装

以下软件包是在编译和运行HAH节点所需的开发工具和依赖项。

* g++：GNU C++编译器，用于编译C++代码。
* libboost-all-dev：Boost C++库的开发文件，提供了许多有用的C++工具和库。
* openssl：用于安全通信的加密库。
* libreadline-dev：用于命令行交互的库，提供命令行编辑和历史记录功能。
* pkg-config：用于检索已安装软件包的元数据和编译选项的工具。
* libncurses5-dev：用于在终端中处理文本界面的库。
* autoconf：用于生成Makefile的工具。

#### 安装环境配置所需的库

打开电脑终端并输入下列命令：

```
$ sudo apt install -y g++ libboost-all-dev openssl libreadline-dev pkg-config libncurses5-dev autoconf
```

ubuntu16.04

```
$ sudo apt install -y libssl-dev
```

ubuntu18.04

```
$ sudo apt install -y libssl1.0-dev libprotobuf-dev protobuf-compiler
```

#### 安装 CMake

```
$ wget https://github.com/Kitware/CMake/releases/download/v3.22.3/cmake-3.22.3.tar.gz
$ tar -zxvf cmake-3.22.3.tar.gz
$ cd cmake-3.22.3
$ ./bootstrap
$ make
$ sudo make install
$ sudo ln -s /usr/local/bin/cmake /usr/bin
$ cmake --version
$ cd ..
```

#### 安装gcc编译器

```
$ sudo apt install build-essential
$ sudo apt install software-properties-common
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
$ sudo apt install gcc-10 g++-10
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 --slave /usr/bin/g++ g++ /usr/bin/g++-10
$ sudo update-alternatives --config gcc
```

#### 安装 libsodium&#x20;

```
$ wget https://github.com/jedisct1/libsodium/releases/download/1.0.18-RELEASE/libsodium-1.0.18.tar.gz --no-check-certificate
$ tar -zxvf libsodium-1.0.18.tar.gz
$ cd libsodium-1.0.18 
$ ./configure 
$ make -j8 && make check 
$ sudo make install 
$ cd .. 
```

#### 安装 Protocol Buffers

```
$ wget https://github.com/protocolbuffers/protobuf/releases/download/v3.11.3/protobuf-cpp-3.11.3.tar.gz 
$ tar -xzvf protobuf-cpp-3.11.3.tar.gz 
$ cd protobuf-3.11.3
$ ./configure --prefix=/usr
$ make -j8 
$ sudo make install
$ Buffers
$ cd ..
```

#### 下载客户端代码并编译

从 Github 上克隆 HashAhead 代码仓库，并进入到 hashahead 目录中，然后执行 INSTALL.sh 脚本来安装 HashAhead 节点。其中 INSTALL.sh 脚本会依据系统环境自动进行依赖安装、编译等操作，以完成节点的安装和配置。

<pre><code>$ git clone https://github.com/Block-Way/HashAhead.git
$ cd hashahead
<strong>$ ./INSTALL.sh
</strong></code></pre>

到此为止，Hash Ahead客户端已安装完毕，接下来进行节点的安装以及编译

#### 配置节点

以下代码是创建 \~/.hashahead 目录，并在该目录下创建 hashahead.conf 配置文件，并使用 vim 编辑该文件。其中，该文件定义了以下内容：

```
$ mkdir ~/.hashahead
$ cd ~/.hashahead
$ vim hashahead.conf
mpvssaddress=1j6x8vdkkbnxe8qwjggfan9c8m8zhmez7gm3pznsqxgch3eyrwxby8eda
mpvsskey=28efbfda61b473c37549d02784648d89fe21ff082b7a42da9ef97b0b83cdb1a9
rewardratio=500
cryptonightaddress=1j6x8vdkkbnxe8qwjggfan9c8m8zhmez7gm3pznsqxgch3eyrwxby8eda
cryptonightkey=28efbfda61b473c37549d02784648d89fe21ff082b7a42da9ef97b0b83cdb1a9
listen4
```

你也可以通过新建hashahead.conf文件来进行节点的配置，可参考[运行节点](yun-hang-jie-dian.md)
