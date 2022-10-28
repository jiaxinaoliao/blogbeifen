---
title: blog快速搭建教程（基于github和gitpages）
tags: blog
abbrlink: 95d688e6
date: 2022-10-19 10:48:42
categories: blog教程
---

## 1. 博客搭建教程

地址是：[贾心奥利奥 (jiaxinaoliao.xyz)](https://jiaxinaoliao.xyz/)

用的`github + github pages`可以说是完全免费了，就是github的话连接不是很稳定

不过免费的要啥自行车，而且最近都可以直连还是比较稳定

缺点就是github.io的后缀在百度上是找不到的，不过不影响访问，问题不大



## 2. 快速搭建教程

### 1. 工具准备

操作非常简单

是在windows环境下搭建的,mac的话原理是一样的

首先要有一个scoop，scoop的安装可以看后面的教程

mac和linux有自己的包管理器



首先用scoop安装git和node.js

```powershell
scoop install git
scoop install nodejs

用
npm -v
node -v
检查版本
输出版本说面安装成功

之后安装hexo
npm install -g hexo-cli
用
hexo -v
查看版本
```



### 2. github仓库搭建

github注册的话就不说了去官网[https://github.com](https://github.com/)一步一步来注册一个账号即可

注册账号之后新建一个仓库

注意仓库的名字（Repository name）要和所有者（Owner）的名字一样比如abc/abc.github.io

abc.github.io后面的github.io是固定的abc要和仓库所有者的名字一样

最后生成的博客网址就是https://abc.github.io/


{% asset_img my-first-blog-1.png %}

下面的描述（Description）可写可不写

可以添加一个.md文件（把下面的Add a README file勾选上即可）

之后点击绿色按钮创建仓库即可



### 3. SSH key

#### 1. 连接ssh

```powershell
在终端中用
ssh-keygen -t rsa -C "电子邮件地址"
这里的电子邮件是github注册时的电子邮件地址
如：ssh-keygen -t rsa -C "abc@qq.com"

```



#### 2. 添加秘钥

在C:\Users\用户名\\.ssh文件夹中（记得要显示隐藏文件才能看见.ssh文件夹）里面有一个`id_rsa.pub`文件用记事本打开全选复制里面的内容之后打开自己的girhub页面

{% asset_img my-first-blog-2.png %}

找到下面的Settings打开

{% asset_img my-first-blog-3.png %}

打开ssh

{% asset_img my-first-blog-4.png %}



{% asset_img my-first-blog-5.png %}



title的话可以写比如my blog之类的没有影响

在key里面粘贴刚才复制的公钥什么都不要改按绿色按钮添加即可



#### 3. 测试是否成功

```powershell
之后在终端用
ssh -T git@github.com
测试是否成功

成功的话会输出
Hi abc! You've successfully authenticated,
之后就可以了
```



## 3. 生成本地内容

新建一个blog文件夹放在哪里都可以用来存放自己的博客的文件

之后在终端中cd到blog文件夹中

如果配置过程中有什么问题直接把blog文件夹删了即可之后再新建一个

```powershell
在文件夹中先进行初始化
hexo init
出现
INFO Start blogging with Hexo!
说明安装成功
安装没有成功的话多半是网络问题多运行几遍命令


可以直接用
hexo s
和hexo server一样的效果
可以去http://localhost:4000/查看网页初始化的页面
用ctrl+c停止
```



## 4. 发布到互联网

在blog文件夹中应该是



{% asset_img my-first-blog-6.png %}

大概这样其中`_config.yml`需要我们自己修改一下

可以用记事本打开将最后面的改为

```powershell
deploy:
  type: git
  repository: https://github.com/abc/abc.github.io.git
  branch: main
```

即可

{% asset_img my-first-blog-7.png %}

在文件的前面位置可以更改title如`title: abc`以及authot作者等等

美化的事情后面再说先将博客搭建起来



更改之后保存关闭之后即可

之后在终端中运行`npm install hexo-deployer-git --save`安装一下这个工具



```powershell
之后cd到blog文件下
hexo generate
和hexo g效果一样
生成页面

之后用
hexo d进行上传
和hexo deploy一样
```



到此基本就完成了你的网址就是https://abc.github.io/

直接用浏览器打开就可以看见了



## 5. 文章的创建

```powershell
可以直接在blog文件夹中用命令行
hexo new "myblog.md"
需要等待几秒
或者直接在blog的文件夹中的source/_posts下面直接创建.md的文件
```

编写好.md文件之后用

```powershell
hexo s
本地预览

hexo g
生成页面

hexo d
推送出去
```

在https://abc.github.io/就可以看见了
