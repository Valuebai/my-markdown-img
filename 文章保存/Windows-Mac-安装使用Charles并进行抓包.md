---
title: Windows-Mac-安装使用Charles并进行抓包
tags:  charles, Windows安装charles, Mac安装charles, charles使用
author:  [Valuebai](https://github.com/Valuebai/)
---
charles是神马？
    一个HTTP代理服务器,HTTP监视器,反转代理服务器·它允许一个开发者查看所有连接互联网的HTTP通信·这些包括request, response现HTTP headers （包含cookies与caching信息），常用于手机（android, ios）抓包，电脑浏览器抓包，弱网测试，重定向等功能，前期的安装对于新人会有点头大，最好问下老鸟。

#### 官网下载地址：
https://www.charlesproxy.com/download/

####  使用教程（windows版）
http://blog.csdn.net/zxz_tsgx/article/details/52635115
	
#####   使用教程（Mac版）
http://blog.devtang.com/2015/11/14/charles-introduction/
	
####   安装流程
#####  0. 安装并激活charles，官网下载后，用下面的激活
- 适用于Charles任意版本的注册码（so，直接去官网下载安装包）：  https://zhile.io/2017/07/07/charles-proxy-usage-and-license.html
- // 适用于Charles任意版本的注册码，谁还会想要使用破解版呢。 
- Registered Name: https://zhile.io License Key: 48891cf209c6d32bf4

#####  1. 打开charles的电脑必须和连接的手机在同一个局域网下
#####  2. 电脑设置代理，手机设备连接charles后有allow提醒，没有的话关掉网络重新来

![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564561815135.png)

#####  3. allow后，再根据android或者ios进行安装证书，安装charles的电脑也要安装证书，有问题的看下面
* 使用手机浏览器（安卓手机推荐用UC打开后可直接安装，苹果手机用safari）打开chls.pro/ssl，打开后一般就会显示证书安装，按照正常流程安装即可
* 苹果手机额外操作：「设置」-「通用」-「关于本机」-「证书信任设置」-开启「Charles Proxy Custom Root Certificate」证书信任。
* 小米安装证书有问题的话，可以参考：https://blog.csdn.net/jinshitou2012/article/details/79044560      https://testerhome.com/topics/9445


#####  4. 电脑的charles设置代理端口，开启443显示https抓包内容5、如果使用笔记本开启热点的，可能因为防火墙开着导致无法显示抓包
* 如果发现手机连接笔记本开启的热点，老是无法抓到包，也没有提醒连接授权，关掉笔记本的防火墙，关掉防火墙，关掉防火墙！！！


####   电脑本地证书的安装


![enter description here](https://user-images.githubusercontent.com/9695113/62196674-0a020a80-b3b1-11e9-8eec-a475eda7a094.gif)
    

####   启用https捕捉
![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564561889763.png)
![enter description here](https://www.github.com/Valuebai/my-markdown-img/raw/master/小书匠/1564562158644.png)

经过以上设置后，即可使用charles抓包https，另外可以使用charles的map映射功能，将生产地址映射到开发测试环境。
    
