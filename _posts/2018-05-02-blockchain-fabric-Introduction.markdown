---
layout:     post
title:      "HyperLedger-Fabric介绍"
subtitle:   " \"Hello Fabric\""
date:       2018-05-02 16:09:00
author:     "TenzT"
header-img: "img/fabric-bg.jpg"
tags:
    - BlockChain
---

> “BlockChain is on the way. ”
## 简介
Fabric是一个区块链的开源框架
本文对Fabric进行简单介绍，重点在了解术语（Terminology）企业需求点

## 参考
- [HyperLedger-Fabric 官方文档](http://hyperledger-fabric.readthedocs.io/en/latest/index.html)
## 正文
1. 什么是fabric？  
fabric 是一个开源的企业级的分布帐平台（DLT，distributed ledger technology）,它通过模块化的架构来允许组件的“插入-运行”来实现这份协议规范。它具有强大的'容器技术'来支持任何主流的语言来开发智能合约。
<br>

2. 企业级应用需求：
- 必须能够确知交易的参与者，而不是匿名的
- 需要和认许制 (Permissioned)结合，即需要限制让哪些节点参与交易验证及存取所有的资料。
- 高交易吞吐量
- 低延迟的交易确认（原始的区块链需通过共识机制打包上链，使得交易确认延迟）
- 需要保证交易和数据的隐私和机密性

3. 为什么是fabric？
早期的区块链技术提供一个目的集合，但是通常对具体的工业应用支持的不是很好。为了满足现代市场的需求，fabric 是基于企业关注点针对特定行业的多种多样的需求来设计的，并引入了这个领域内的开拓者的经验，如扩展性。fabric 为权限网络，隐私，和多个区块链网络的私密信息提供一种新的方法。
<br>


4. 关键术语
- 交易(Transaction) 是区块链上执行功能的一个请求。功能是使用链节点(chainnode)来实现的。
- 交易者(Transactor) 是向客户端应用这样发出交易的实体。
- 总账(Ledger) 是一系列包含交易和当前世界状态(World State)的加密的链接块。
- 世界状态(World State) 是包含交易执行结果的变量集合。
- 链码(Chaincode) 是作为交易的一部分保存在总账上的应用级的代码（如智能合约）。链节点运行的交易可能会改变世界状态。
- 验证Peer(Validating Peer) 是网络中负责达成共识，验证交易并维护总账的一个计算节点。
- 非验证Peer(Non-validating Peer) 是网络上作为代理把交易员连接到附近验证节点的计算节点。非验证Peer只验证交易但不执行它们。它还承载事件流服务和REST服务。
- 带有权限的总账(Permissioned Ledger) 是一个由每个实体或节点都是网络成员所组成的区块链网络。匿名节点是不允许连接的。
- 隐私(Privacy) 是链上的交易者需要隐瞒自己在网络上身份。虽然网络的成员可以查看交易，但是交易在没有得到特殊的权限前不能连接到交易者。
- 保密(Confidentiality) 是交易的内容不能被非利益相关者访问到的功能。
- 可审计性(Auditability) 作为商业用途的区块链需要遵守法规，很容易让监管机构审计交易记录。所以区块链是必须的。

5. 开发语言
核心开发语言是GoLang，支持Java，Node.js开发
