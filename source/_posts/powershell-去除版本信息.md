---
title: powershell 去除版本信息
tags: powershell
categories: 编程入门
abbrlink: 60075
date: 2022-10-26 13:52:08
---

# 去除powershell的版本信息

正常打开powershell的话会显示版本信息以及版权

{% asset_img pwsh-1.png %}

要想去除的话只要在windows terminal的设置中

{% asset_img pwsh-2.png %}

点击powershell的配置，打开右侧的命令行在后面加入`空格-nologo`即可

{% asset_img pwsh-3.png %}

记得保存重新打开即可

{% asset_img pwsh-4.png %}

再次打开就没有了



**注意：**用oh-my-posh的话会有一些慢并且在vim里面打开的话还是默认有版本信息的，不建议去掉

去掉的话美化效果会出问题，对功能没有影响

{% asset_img pwsh-5.png %}

会出现奇奇怪怪的样子不过只要clear一下就可以了，

不去掉的话不会出现这个问题

{% asset_img pwsh-6.png %}

