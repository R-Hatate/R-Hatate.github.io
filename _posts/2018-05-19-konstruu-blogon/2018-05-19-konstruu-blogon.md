---
layout: post
title: konstruu blogon
category: 信息学竞赛
tags: [如何搭建XXX, 博客]
---

# 大致流程

搭建一个个人博客（这里指的是使用$CMS$的，而非静态博客），大概你需要准备如下东西：

1. 一个域名
2. 一个主机

# 先去搞一个域名

有两种选择方案，花钱和不花钱

## 用$tk$域名

在[dot.tk](http://www.dot.tk)中可以申请一个免费域名，最长一年，超期之后需要花钱续费

## 用$pw$域名

在[gandi.net](https://www.gandi.net/zh-hans)中可以申请一个$pw$域名，第一年只需要$3$块钱，第二年只需要$30$块钱，接下来每年需要$82$块钱，是不是很良心

购买之后你需要去[Dashboard](https://admin.gandi.net/dashboard/)中配置域名的解析

在**域名->xxx.pw->区域档记录**中将$CNAME$解析到[pages.coding.io](pages.coding.io)，然后等待解析成功

如果解析成功的话，应该会显示[coding.net](coding.net)的相关界面，然后你可以去准备主机了！

# 准备一个主机

有两种选择方案，花钱和不花钱

## 去枫叶主机购买

[枫叶主机](https://www.fyzhuji.com/aff.php?aff=4722)是一个价格良心的主机商，[香港主机](https://fyzhuji.com/hk.html)一年才要$60RMB$

## 用[coding.net](coding.net)提供的免费主机服务

> update(2018-06-09): CODING的动态pages页面显示了这么一段话：
> 
>> 动态 Pages 将在 6.30 日下线，建议您使用 Cloud Studio ，一键开启快速开发部署云时代！

先去[coding.net](coding.net)注册一个账户，然后[升级成为银牌会员](https://coding.net/user/account/upgrade)（其实就是把你[个人设置](https://coding.net/user/account/setting/basic)中带$*$的地方补充完整就好）

本章中介绍$Typecho$的搭建过程

然后去[这里](https://coding.net/u/KingSann/p/typecho_kod/git)$Fork$一下该项目，你可以在[这里](https://coding.net/user)找到你$Fork$掉的那个项目，点进去之后去**$Pages$服务**里点**动态$Pages$**，并开启

在**部署来源**中选择**$master$ 分支**，并保存，并关闭**自动部署**，并等待部署完毕

部署成功后将你的域名在**自定义域名**中添加（你需要先解析好域名）

之后在浏览器中输入你的域名，进去之后点击**开始下一步**

返回刚才的界面，在**存储空间**中打开**链接信息**，用这些信息进行配置网站基础设置

$Typecho$ 中的设置大致如下（结合着**链接信息配置**）

- **数据库适配器**：$Pdo$驱动$Mysql$适配器
- **数据库地址**：[mysql.coding.io](mysql.coding.io) 在**链接信息配置**中（一般来说不用管）
- **数据库端口**：$3306$ 在**链接信息配置**中（一般来说不用管）
- **数据库用户名**：$user-xxxxxxx$ 在**链接信息配置**中
- **数据库密码**：$xxxxxxx$ 在**链接信息配置**中
- **数据库名**：$db-xxxxxx$ 在**链接信息配置**中
- **数据库前缀**：这个不用管
- **网站地址**：你的域名
- **用户名**：管理你的网站要用的用户名
- **登录密码**：该用户名对应的密码
- **邮件地址**：你的邮箱，可以使用域名邮箱

然后确认安装就安装好咯，不过有概率会显示数据库无法连接，不过概率大概很小(?)罢了

# 扩展

虽然你现在有一个自己的博客了，但是你会发现有一些问题

## 访问速度好慢啊

推荐[cloudflare](cloudflare.com)的$cdn$加速服务，而且甚至可以支持全站$https$（不过似乎只是用户到$cloudflare$是$https$）

如果开启了全站**https**的话，还需要在$Typecho$中做一些设置，使得$Typecho$相关调用**http://**的位置都改成**//**

## 没法知道每天的浏览量啊

推荐[腾讯统计](http://ta.qq.com/)，配置好之后支持查看

- 访问趋势
- 访客地域
- 关键词
- 性别比例
- 页面排行
- 访问深度
- 全部来源
- 搜索引擎收录
- 网站可用性
- 入口页面排名
- 外部链接进入排名

## 好像没几个人看博客啊

这涉及到了$SEO$各种玄学优化，我不会

## 换个好看一些的模板

这个百度**$Typecho$模板**就好

## $bgm$

[网易云](http://music.163.com)的外链播放器功能

## $\LaTeX$支持

手动改模板的相关代码

## 一言

让你的网站更[文艺](https://hitoapi.cc/)

# 其他

$Wordpress$相关的方面可以去看[hzwer](http://hzwer.com/7560.html)的博客