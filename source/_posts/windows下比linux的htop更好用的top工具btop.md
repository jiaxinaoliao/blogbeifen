---
title: windows下的top工具--btop
tags: windows终端插件分享
categories: 编程入门
abbrlink: 34163
date: 2022-11-11 12:18:58
---



一款炫酷的性能测试监控分析工具——btop

linux的top命令，作为我们性能分析的入门命令，一般都是大家首先使用的。但是，top命令的结果，只有和白色，颜色很单一，linux下有htop

windows下怎么达到同样的效果呢，可以通过scoop安装btop

btop，可以通过不同颜色、显示处理器、内存、磁盘、网络和进程的使用情况和统计信息的资源监视器。C++[bashtop](https://github.com/aristocratos/bashtop)和[bpytop](https://github.com/aristocratos/bpytop)的版本和延续。



在windows下直接用scoop安装即可`scoop install btop`



{% asset_img btop-1.png %}



| m           | 打开选项参数                                   |
| ----------- | ---------------------------------------------- |
| o           | 进入窗口颜色、数据刷新频率等相关配置           |
| h           | 打开帮助                                       |
| ctrl + z    | 停止进程                                       |
| ctrl + c、q | 退出                                           |
| -           | 减少页面数据刷新间隔，默认2s秒                 |
| +           | 增加页面数据刷新间隔                           |
| f，/        | 开启进程搜索模式，输入搜索关键词，可以搜索进程 |
