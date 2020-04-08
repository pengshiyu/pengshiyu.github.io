---
title: Charles4.0.2在Mac环境安装及使用
date: 2020-03-29 18:44:03
tags:
    - Charles
categories:
    - 后端
---

![](/uploads/WX20200329-184509@2x.png)

# 安装
版本：4.20
下载地址：https://pan.baidu.com/s/1c22zTWO    密码：6mu3

备用链接: https://pan.baidu.com/s/1Q51aoOb5KGoS8mBtOK_QQQ 
提取码: dx5e 

<!-- more  -->

Charles的破解方法：
1.打开dmg镜像，将“Charles.app”拖入应用程序中；
2.打开应用程序—右键“Charles.app”显示包内容—Contents—Java；
3.将dmg镜像包内的“charles.jar”替换覆盖到第二步的Java文件夹中；
4.打开“Charles.app”，等待10秒，菜单栏中找到“Help”—“Register Charles…”，输入任意信息完成注册；
5.完成
系统版本要求：OS X 10.7 或更高。

# 乱码问题
下载 CA 证书文件安装ssl证书：
https://www.charlesproxy.com/documentation/additional/legacy-ssl-proxying/

具体参考本文末尾文章

1、安装证书
上方菜单栏 —-》Help —-》SSL Proxying —-》Install Charles Root Certificate
并信任证书

2、设置抓取端口
上方菜单栏 —-》Proxy —-》SSL Proxy Settings —-》Add
Host：填*表示所有网站都抓 
Port：443 

参考：
[Mac Charles乱码解决办法](https://blog.csdn.net/a327369238/article/details/52856833)

# 手机抓包
## iphone： 
1. [使用Charles对iPhone进行Http(s)请求拦截(抓包)](https://www.jianshu.com/p/595e8b556a60?from=timeline&isappinstalled=0)
## android： 
1. [Mac下使用Charles进行Android抓包](https://jingyan.baidu.com/article/495ba841c49f1438b30ede90.html)
2. [如何给华为手机网络连接设置代理](https://jingyan.baidu.com/article/db55b609dbf04f4ba30a2fb2.html)
3. [华为怎么安装证书](https://jingyan.baidu.com/article/ad310e800d47361849f49efd.html)

>参考：
[Charles 4.0.2 Mac破解版](http://www.sdifen.com/charles402.html)
[解决Charles乱码问题](https://www.jianshu.com/p/ddef21c3b9ba)