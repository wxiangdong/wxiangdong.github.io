---
title: hexo多终端同步管理
date: 2018-06-23 15:24:58
categories: "Hexo教程"
tags: 
     - hexo
---

前几天使用hexo搭建了Github博客，今天在家里的电脑上想要同步Github博客到本地，同时达到备份博客主题、文章、配置时，遇到了点坑，查询了 一下网上的资料，现在记录一下，也算给遇到同样问题的小伙伴们一个参考。

<!-- more -->

## 创建新分支

- hexo使用命令`hexo g`(生成静态文件)和`hexo d`(部署到github)，这些生成的文件会放在master分支上，如图：
{% asset_img root_config.png %}

- 所以我们新创建hexo分支，将Hexo配置写博客用的相关源文件放在hexo分支上，多终端的同步只需要对分支hexo进行操作
{% asset_img create_branch.png %}

- 将hexo分支设为默认分支
{% asset_img default_branch.png %}

## 备份

1. 将刚刚创建的新仓库clone到本地，`git clone https://github.com/yourname/yourname.github.io`,将之前的blog中的`_config.xml`,`themes`,`sources`,`scaffolds`,`package.json`,`.gitignore`,复制到yourname.github.io文件夹中;
2. 将themes/next/(我用的是NexT主题)中的.git/删除，否则无法将主题文件夹push；
3. 在yourname.github.io文件夹执行`npm install`和`npm install hexo-deployer-git`（这里可以看一看分支是不是显示为hexo）；
4. 执行`git add .`、`git commit -m ""`、`git push origin hexo`提交hexo网站源文件到github
5. 执行`hexo g -d`生成静态网页部署至github

这样一来yourname.github.io仓库就有master分支和hexo分支，分别保存静态网页和源文件


## 恢复

在其他电脑上修改博客:
1. 安装node.js和npm
2. 安装git
3. 执行`git clone https://github.com/yourname/yourname.github.io`将仓库clone到本地
4. 在文件夹内执行以下命令`npm install hexo-cli -g`、`npm install`、`npm install hexo-deployer-git`

> 注意: 千万不要执行`hexo init`，不然之前的hexo配置都会被重置


## 修改

每次在要编辑博客之前，执行`git pull`，保证本地和服务器同步
在本地对博客修改（包括修改主题样式、发布新文章等）后：
  1. 依次执行`git add .`、`git commit -m ""`、`git push origin hexo`来提交hexo网站源文件；
  2. 执行`hexo g -d`生成静态网页部署至Github上。

即重复备份的7-8步骤

好了，到这里多终端同步管理就完工了，如若文中有什么地方有错，还请留言告知，谢谢！

参考:
[如何解决github+Hexo的博客多终端同步问题][1]
[Hexo搭建博客并实现多终端同步管理][2]
[Hexo博客的跨设备同步][3]



[1]: https://blog.csdn.net/Monkey_LZL/article/details/60870891
[2]: https://www.imooc.com/article/9707?block_id=tuijian_wz
[3]: https://www.jianshu.com/p/6fb0b287f950