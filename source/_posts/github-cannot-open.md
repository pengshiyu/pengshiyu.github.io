---
title: Github不能正常访问的解决方法
date: 2020-03-29 18:41:59
tags:
    - Github
---

## 问题：
1、Github能打开，图片不能正常显示
2、Github代码推送超时

<!-- more -->

## 解决：
hosts文件添加ip映射
```
windows下路径：
C:\Windows\System32\drivers\etc\hosts

Linux下路径：
/etc/hosts
```

加入以下内容
```
# GitHub Start

192.30.253.112 github.com

192.30.253.119 gist.github.com

151.101.100.133 assets-cdn.github.com

151.101.100.133 raw.githubusercontent.com

151.101.100.133 gist.githubusercontent.com

151.101.100.133 cloud.githubusercontent.com

151.101.100.133 camo.githubusercontent.com

151.101.100.133 avatars0.githubusercontent.com

151.101.100.133 avatars1.githubusercontent.com

151.101.100.133 avatars2.githubusercontent.com

151.101.100.133 avatars3.githubusercontent.com

151.101.100.133 avatars4.githubusercontent.com

151.101.100.133 avatars5.githubusercontent.com

151.101.100.133 avatars6.githubusercontent.com

151.101.100.133 avatars7.githubusercontent.com

151.101.100.133 avatars8.githubusercontent.com 

```
> 参考
> [Github之前进入正常，随后访问不了解决方案](https://www.jianshu.com/p/4ca8fd6d6b16)