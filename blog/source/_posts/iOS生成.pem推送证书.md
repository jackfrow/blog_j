---
title: iOS pem 证书生成
date: 2019-1-15 16:52:59
tag: iOS
categories: 学习笔记
---

# pem文件概述

​	pem文件是服务器向苹果服务器做推送时候需要的文件，主要是给php向苹果服务器验证时使用，下面介绍一下pem文件的生成。

# 生成.pem的方法

​	生成pem主要有两种方式,1种是分别生成.pem的cert和生成.pem的key,然后再合成服务器需要的.pem。另一种是直接生成服务器需要的.pem。

## 分别生成

### 生成cert文件

![F9EE8275-559C-496A-8B9D-57E6C0DD2308.png](https://i.loli.net/2019/01/16/5c3ea96bedbdb.png)

![C4B31488-5045-44C0-AE65-532E13E210B5.png](https://i.loli.net/2019/01/16/5c3ea96b7ee4a.png)

![8CADA519-4E76-4242-A40F-58A6FF9BA487.png](https://i.loli.net/2019/01/16/5c3ea96a5cf94.png)



### 生成key文件

![3185DA78-5055-40AA-A8AD-6A2487AEAC7F.png](https://i.loli.net/2019/01/16/5c3ea96bd197e.png)

![5CCCADF1-8D36-455B-A5E8-F1D9E6385AB6.png](https://i.loli.net/2019/01/16/5c3ea96a4926e.png)

![1C5B5045-8DC1-4A7F-A822-B8A76549A535.png](https://i.loli.net/2019/01/16/5c3ea96a30943.png)



### 生成cert pem文件​	

​	将apns-dev-cert.p12文件转换为pem格式

```
openssl pkcs12 -clcerts -nokeys -out apns-dev-cert.pem -in apns-dev-cert.p12
```



### 生成key pem文件

​	将apns-dev-key.p12文件转换为pem格式

```
openssl pkcs12 -nocerts -out apns-dev-key.pem -in apns-dev-key.p12
```

​	移除key pem的密码

```
openssl rsa -in apns-dev-key.pem -out apns-dev-key.pem
```



### 合成推送需要的pem文件

​	将apns-dev-cert.pem和apns-dev-key.pem文件合成为apns-dev.pem文件

```
cat apns-dev-cert.pem apns-dev-key.pem > apns-dev.pem
```



### 测试pem文

在终端测试：

```
openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert apns-dev-cert.pem -key apns-dev-key.pem
```

终端最后显示以下内容，表示配置pem文件成功，

Key-Arg   : None

Start Time: 1467854873

Timeout   : 300 (sec)

Verify return code: 0 (ok)

## 直接生成

​	**同时**选中cert和key,导出一份p12文件。

![QQ20190116-102210@2x.png](https://i.loli.net/2019/01/16/5c3ea96ed3b56.png)

​	将p12文件转化成后台需要的pem证书.

```
openssl pkcs12 -in push_hilife.p12 -out pushcert.pem -nodes -clcerts
```

### 工具

​	平时涉及到推送都不太好测试，这时候怎么办呢，有网友给出了解决方案，使用它们开发好的工具可以调试推送。

[*Knuff*](https://github.com/KnuffApp/Knuff)

[*SmartPush*](https://github.com/shaojiankui/SmartPush)

## 参考文章

[iOS推送证书生成pem文件（详细生成过程）](https://www.jianshu.com/p/cc952ea07a08)

[Generate .pem file Used to setup Apple Push Notification](https://stackoverflow.com/questions/21250510/generate-pem-file-used-to-setup-apple-push-notification)

