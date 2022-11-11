---
title: windows下的包管理器scoop
date: 2022-11-11 12:34:37
tags: windows终端插件分享
categories: 编程入门
---

### 1 Scoop包管理器

scoop是windows下的一个强大的包管理器可以方便快速的安装软件



#### 2 scoop安装

安装scoop很简单只需要在powershell中输入一条指令

`iwr -useb get.scoop.sh | iex`即可自动安装

但是默认是在C盘安装所以在运行之前先要更改地址

并且要让powershell可以执行脚本需要输入

`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

回车即可

之后依次输入下面的**两条指令**'D:\ScoopApp\Scoop'单引号内部的地址可以更改为自己的地址其他的不要动

```powershell
$env:SCOOP='D:\ScoopApp\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
```

第一条指令没有问题第二条指令也一样单引号内的地址可以更改可能需要管理员身份右键wt以管理身份运行即可（报错也没有关系可以忽略）

```powershell
$env:SCOOP_GLOBAL='D:\ScoopApp\GlobalScoopApps' [Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL,'Machine')
```

输入上两条指令之后再输入`iwr -useb get.scoop.sh | iex`即可开始安装scoop了

安装完成之后在'D:\ScoopApp\Scoop'（这里是刚刚自己选择的地方）的地方会有一个scoop的文件夹里面会有

- apps——所有通过scoop安装的软件都在里面。
- buckets——管理软件的仓库，用于记录哪些软件可以安装、更新等信息，默认添加`main`仓库，主要包含无需GUI的软件，可手动添加其他仓库或自建仓库，具体在[推荐软件仓库](https://zhuanlan.zhihu.com/write#推荐软件仓库)中介绍。
- cache——软件下载后安装包暂存目录。
- persit——用于储存一些用户数据，不会随软件更新而替换。
- shims——用于软链接应用，使应用之间不会互相干扰，实际使用过程中无用户操作不必细究。





scoop有很多功能可以自行上网查看，这里只用几个最基本的需要用到的

- install——安装软件。
- uninstall——卸载软件。
- update——更新软件。可通过`scoop update *`更新所有已安装软件，或通过`scoop update`更新所有软件仓库资料及Scoop自身而不更新软件。

比如需要安装git只需要在powershell中输入命令`scoop install git`即可

scoop + 命令 + 软件名称

 卸载的话就用`scoop uninstall git`

还有一个search命令用来查找比如我想安装一个top可以先用`scoop search top`来查看那些包含top

比如找到了btop就可以用`scoop install btop`来安装了



{% asset_img scoop-1.png %}
