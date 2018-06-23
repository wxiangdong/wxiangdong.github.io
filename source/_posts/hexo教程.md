---
title: hexo教程
date: 2018-06-21 09:47:51
categories: "Hexo教程" 
tags: 
     - javascript
     - hexo
---

前几天偶然看到一个朋友blog，感觉很流弊的样子，便在网上查找了一些[hexo][1]和[Github][2]搭建静态博客的教程，加上之前在万网购买的.xyz域名，于是我的个人blog诞生了。
以下是在windows环境操作

-----------------

<!-- more -->

## 安装node.js

首先登录[node.js官网][3]，选择适合自己的版本下载，然后安装。(你也可以参考hexo官网的方法)


## 安装Git

登录[git官网][4]选择版本下载，一路next即可完成安装。
> 注意: 安装过程中，注意勾选在右键菜单建立git bash快捷方式一项，因为之后的各种操作都需要在git bash中用命令行进行操作，方便随时随地打开命令窗口。


## 安装Hexo

在电脑任意空白处点击右键，选择Git Bash Here打开命令行
> npm install -g cnpm --registry=https://registry.npm.taobao.org

然后就可以使用下面的命令从npm安装Hexo：
> npm install hexo-cli -g


## 初始化Hexo
安装完成之后，就可以选择一个自己的文件夹作为博客的根目录( 例如 D:\blog ),然后在该目录下打开命令行
> hexo init

初始化博客空间，生成博客运作所需要的文件，接下来安装依赖包
> npm install

此时，你的文件夹下的目录结构应该是这样：
![](/images/hexo_init.png)

## 本地运行
> hexo g
> hexo s

在浏览器地址栏输入[http://localhost:4000/][5],按下回车键，准备好迎接美丽的新领域吧。


## Hexo的一些基本命令
> hexo g #完整命令为hexo generate,用于生成静态文件
> hexo s #完整命令为hexo server,用于启动服务器，主要用来本地预览
> hexo d #完整命令为hexo deploy,用于将本地文件发布到github等git仓库上
> hexo n "my article" #完整命令为hexo new,用于新建一篇名为“my article”的文章


## 发布一篇新文章

首先Ctrl+C停止当前的本地服务，然后
>hexo n "my article"

这样就会在博客目录下source\_posts中生成相应的 我的第一篇文章.md文件( 例如 D:\blog\source\_posts\my article.md )

编辑完成后就可以进行本地预览
> hexo g
> hexo s

## 总结

到目前为止利用Hexo搭建本地环境已经完成，下一节将要开始把博客发布到github上，并利用GitHub Pages完成博客的部署。

[1]: https://hexo.io/
[2]: https://github.com/
[3]: https://nodejs.org/en/
[4]: https://git-scm.com/
[5]: http://localhost:4000/