---
title: arch linux关机遇到(A stop job is running for ...)
tags: arch linux安装教程
categories: arch linux
abbrlink: 6196
date: 2023-05-29 18:09:04
---

# arch linux
arch linux在关机重启的时候经常遇到(A stop job is running for ...)
等待很长时间都没有办法关闭，导致关机很慢。

编辑`/etc/systemd/system.conf`文件找到并修改


```shell
DefaultTimeoutStartSec=10s
DefaultTimeoutStopSec=10s

之后保存退出执行

systemctl daemon-reload
```

