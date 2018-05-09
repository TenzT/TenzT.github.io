---
layout:     post
title:      "HyperLedger-Fabric介绍"
subtitle:   " \"Hello Fabric\""
date:       2018-05-08 20:09:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - BlockChain
    - HyperLedger-Fabric
---

> “BlockChain is on the way. ”


## 简介
本篇详细介绍Fabric v1.0的架构，并配合自己的理解

## 参考
- [HyperLedger-Fabric 官方文档](http://hyperledger-fabric.readthedocs.io/en/latest/index.html)
- 《区块链技术进阶与实践》


## 正文
1. Fabric总体逻辑架构
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/LogicalArchitecture.png)
- Fabric的架构主要由三部分构成：
    1. 成员服务：管理成员，进行权限管理。
    2. 区块链服务：Fabric网络的核心，区块链的主体，包括P2P网络，共识服务，分布式账本和账本存储四部分。
    3. 链码服务：提供接口，使得成员间的交易可编程，如编写智能合约等。
<br>

2. 成员服务
- 成员服务为Fabric的参与者提供网络上的身份管理、隐私、保密性和可审核性的服务。
- 该服务的实现方式为PKI体系，可实现不同成员在不见面的情况下进行安全通信。<font color=#FF4500 size=3 face="黑体">在成员管理上，不可避免的需要引入中心化的结构，Fabric当前采用的模型是基于可信的第三方机构。</font>基于这一体系，Fabric将非许可链转变成一个许可区块链。
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/sec-memserv-components.png)
- 成员服务由几个基本实体构成：
    1. Root Certificate Authority(Root CA)：绝对被信任的实体，整个PKI中最顶层的认证机构。
    2. Enrollment Certificate Authority(ECA)：验证用户，发放注册证书(ECerts， 个人理解为成员的身份证，每个证书与一个成员绑定，用于权限管理，加入移除网络等)。
    3. Transaction Certificate Authority()：验证用户，发放交易证书即交易凭证，可用于验证交易。
    4. TLS Certificate Authority(TCA)：使用网络的[TLS](https://segmentfault.com/a/1190000007075961)证书。
    5. Enrollment Certificates(ECerts)：身份证书，与成员绑定。
    6. Transaction Certificates(TCerts)：用于交易的短期证书，可被配置为不携带用户身份的信息。
    7. TLS-Certificates(TLS-Certs)：系统和组件之间通信的证书，携带所有者的信息。
    8. Code Signer Certificates(CodeSignerCerts)：对代码（目前理解为链码）进行签名。
<br>

2. 区块链服务（核心）
- P2P网络：原始的区块链的网络结构，去中心化的核心。在Fabric v0.6下，整个网络只有验证节点和非验证节点两种类型，而v1.0下整个网络的节点类型三个：
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/P2P.png)
    2.1 客户端节点(Client)：直接与实际用户交互，与Peer节点通信，提交实际交易调用，与共识服务请求广播交易。
    2.2 Peer节点：负责维护和更新世界状态（即总账本），接收Client节点发起的交易请求，面向共识网络接收网络广播的喜爱哦系，并以区块的方式接收排序好的交易信息。由于每个合约代码都可以指定一个包含多个背书节点集合的背书策略，Peer节点也可以充当背书节点。
    2.3 公式服务节点(Orderer)：共识网络的核心，提供交付保证。
    - 多通道服务：Fabric提供通道服务（私有链），通道由制定的节点集合构成，一个通道共享一个账本。
- 共识服务：由Orderer节点的聚合形成，可根据不同的应用场景部署不同共识选项。
- 分布式账本：由通道中的Peer节点维护和更新，包括Chain数据库和状态数据库，其中Chain部分存储着所有交易的信息，只可添加查询，不可修改，state部分存储着交易日志中所有变量的最新值。(https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/Leadger.png)
- 个人理解：Orderer是P2P网络的核心，负责共识服务，而Peer节点则负责存储交易数据，协调交易请求，相当于用户参与整个交易市场的“代理人”（如下图所示），Client只需简单的负责发起请求即可。Fabric把区块链中的单个节点负责的任务分散到三种节点上，是各部分可模块化，可插拔的关键，与此同时，用户和共识服务的剥离便于成员管理。
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/RunTimeArchitecture.png)
<br>
3. 合约代码
- 合约代码是区块链上运行的一段代码，事故Fabric中只能合约的实现部分。
- 交易是区块链上流通的实体单元，而交易是通过调用合约代码对应的的操作产生的。于用户而言，是制定交易形式（即合约）的方式。
- 在Peer上部署、安装、调用，用户发起交易则是通知Peer节点来掉用合约代码。
- 支持Java和Go语言编写。