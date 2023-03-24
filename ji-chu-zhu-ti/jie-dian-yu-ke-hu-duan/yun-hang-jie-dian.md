# 运行节点

运行节点之前，你需要进行客户端环境的编译，在客户端通过编译之后，才可进行节点的运行。客户端环境编译的流程请参考客户端相关文档。

如果你是新手，请先了解全节点的作用，通过该文章你将会逐步熟悉Hash Ahead

1\.      在你的用户目录下创建一个目录

<pre><code><strong>$ mkdir -p /home/[user-name]/hashahead
</strong></code></pre>

2\.      在上述文件夹中创建一个配置文件hashahead.conf

```
cryptonightaddress=0x23a100c201e312d1ebecf93545de3d699296ada7
cryptonightkey=0xfc12e97e0d090f5d71d93e28a1defdc4dbeea6de6a53519acdc2611d63addb4b
 
mpvssaddress=0x9835c12d95f059eb4eaecb4b00f2ea2c99b2a0d4
mpvsskey=0xd0aeb58241372a8793dcb43fd4bb3092a0917ba55dab53a64f2bc34baefb1463
rewardratio=500
 
listen4
rpcallowip=*
statdata
fulldb
```

并将上述内容复制到hashahead.conf文件中并保存

3\.      运行Hash Ahead节点

```
$ hashahead -demon
```

4\.      进入Hash Ahead节点

```
$ hashahead -cli
```

返回：

```
hashahead>
```

这时候你已经成功运行一个节点了，请尝试导入你的私钥进行查询



## 使用二进制版本运行完整节点

[https://github.com/Block-Way/HashAhead/releases](https://github.com/Block-Way/HashAhead/releases)

打开链接下载并运行即可
