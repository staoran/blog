---
title: Charles 注册、 SSL 与 Android
date: 2022-04-12 18:06:06
updated: 2022-04-12 18:06:12
categories:
  - [学习笔记, 工具使用]
tags:
---

在这个全面 https 的时代，抓包工具注定绕不开与 SSL 相关的配置，特别是移动端的 app 抓包。
直入正题，首先点击 Help - SSL Proxying - Install Charles Root Certificate 安装证书。导入时选择存储到 本地计算机 - 手动浏览 选择 受信任的根证书颁发机构 。
然后在 Proxy 菜单中查看是否启用 Windows Proxy 。点击 SSL Proxying Settings... ，勾选 Enable SSL Proxying ， 并将 需要抓包的域名 加入 Include （支持通配符，如 \*.qq.com，端口可以不填）。
最后检查 Proxy Settings 是否配置，勾选 Support HTTP/2 和 Enable transparent HTTP proxying。
在未完成上面的配置前，遇到过两个错误，

1. SSL Proxying not enabled for this host: enable in Proxy Settings, SSL locations
1. You may need to configure your browser or application to trust the Charles Root Certificate. See SSL Proxying in the Help menu.

第一个问题是因为未将域名 加入 Include 。
第二个问题是因为未在本机安装证书。
以上就是我的全部配置，Charles 版本是 4.6.2。
下面的东西不多说，懂得都懂。
Registered Name: https://zhile.io
License Key: 48891cf209c6d32bf4
