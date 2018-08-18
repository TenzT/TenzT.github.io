---
layout:     post
title:      "在Mac上运行Fabric掉坑记录"
subtitle:   "e2e_cli"
date:       2018-07-26 17:43:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
	- blockchain
	- HyperLedger-Fabric
---

# 在Mac上运行Fabric掉坑记录
1. gopath问题：
应该在.bash_profile文件里写好gopath的位置(比如说我的是$HOME/go),然后fabric的git下到go/src/github.com/hyperledger里面
2. git版本：
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/fabric/DEBU003.png)
出现这种错误，是因为下载了git master的版本，实际应为release版本再checkout，参考[1]
```
git clone -b release-1.0 https://github.com/hyperledger/fabric  
```

3. network e2ecli_default not found
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/fabric/DEBU006.png)
e2e_cli目录是固定的，启动后创建一个docker network以此为名字，如果修改该目录，要修改e2e_cli/base/peer-base.yaml里面的
```
CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=e2e_cli_default
```

这里神坑，原来写的是e2ecli_default

## 一定要注意的问题
`docker镜像、fabric版本一定要对应，否则容易出各种奇怪的错误`


# 参考资料
- [1] <a href=http://www.rendoumi.com/fabric-1-0zai-centos-7shang-mian-de-an-zhuang>Fabric 1.0在CentOS7上面安装</a>
- [2] https://blog.csdn.net/zzqw199012/article/details/80106267
