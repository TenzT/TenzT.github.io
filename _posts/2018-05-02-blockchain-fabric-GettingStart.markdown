---
layout:     post
title:      "Fabric初体验
subtitle:   " \"Seeing is Believing\""
date:       2018-05-02 19:55:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - BlockChain
---

> “Learning By Doing. ”

## 简介
基于配置好的Docker等环境,使用Fabric 1.1.0官方提供的例程

## 参考
- [HyperLedger-Fabric 官方文档](http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html#docker-and-docker-compose)

# 运行环境
- Ubuntu 16.04（本机）
- Docker Docker version 17.06.2-ce 及以上
- Docker Compose version 1.14.0 及以上
- Go version 1.9.x

## 正文
1. 删除原有关于fabric的images
```docker rmi $(docker images | grep hyperledger) ```
2. 在需要存放Fabric例程的目录作为新的Repo，在该目录下执行
    ```curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0```
下载并解压所有与平台有关的二进制可执行文件。

3. 将fabric-samples/bin的路径加入环境变量（可选）：在~/.bash_profile里添加
    ```
    export PATH=<fabric-samples的路径>/bin:$PATH
    source ～/.bash_profile
    ```

4. 执行以下语句来下载对应的Docker镜像
    ```./<fabric-samples的路径>/bin/get-docker-images.sh```

5. 执行byfn例程
    ```./<fabric-samples的路径>/first-network/byfn.sh```
    - 查看对应源代码可见，该例程的运行环境是 4. 中的Docker镜像：
    ![](../img_markdown/20180503-Introduction-images.bmp)
    - 而脚本是在CLI中调用对应的二进制执行文件来建立网络：
    ![](../img_markdown/20180503-Introduction-byfn.bmp)


6. 总结：
使用fabric有两种方式：
- 一种是使用API，支持Java，node.js，Go。
- 另一种是CLI模式，在命令行调用二进制文件搭建网络。