---
title: hexo入门(二):关联github
date: 2018-06-21 10:34:51
categories: "Hexo教程" 
tags: 
     - javascript
     - hexo
     
---

前篇文章描述了如何搭建hexo本地环境，本篇文章内容记录如何把博客发布到github上，并利用GitHub Pages完成博客的部署

<!-- more -->

嗯，相信大家都有github账号了，如果还没有的小伙伴赶快创建一个吧。

## 创建一个新的仓库

{% asset_img new_repository.png %}

> 注意: Respository name 中的 username.github.io 的 username 一定与前面的 Owner 保持一致，例如我的是wxiangdong.github.io

## 配置Github

现在我们需要编辑blog文件夹下的_config.xml文件(根目录下的)，与自己 Github 账号的 Repository 仓库建立关联。
通过编辑器打开,修改如下内容
{% asset_img root_config.png %}

将其中的username修改成你自己的，记得保存，注意配置的键值对之间一定要有空格。

## 自动部署

配置文件修改完成以后，执行命令，完成自动部署：
```
hexo clean
hexo g
hexo d
```
这样就将你的博客上传至你的github仓库中，可以进入你的github账户查看。在浏览器中输入 username.github.io预览

## hexo绑定域名

我的hexo博客是托管在github上的，每次访问都要使用githubname.github.io这么长的域名来访问，所以我在[万网][1]上买了一个.xyz的域名

- 点击对应的解析设置
{% asset_img yunming_jiexi.png %}

- 点击添加记录，记录类型选A或CNAME，A记录的记录值就是ip地址，github(官方文档)提供了两个IP地址，192.30.252.153和192.30.252.154，这两个IP地址为github的服务器地址，两个都要填上，解析记录设置两个www和@，线路就默认就行了.

{% asset_img record.png %}
- 在source文件夹里创建CNAME文件，不带任何后缀，里面添加你的域名信息，如：wangxiangdong.xyz

{% asset_img c_name.png %}
> 注意: 最好不要添加www.wangxiangdong.xyz，如果添加了就只能用www.wangxiangdong.xyz访问了，如果填写的是wangxiangdong.xyz，那么用www.wangxiangdong.xyz和wangxiangdong.xyz访问都是可以的

然后清理hexo,发布试试吧
```
hexo clean
hexo d -g
```


[1]: https://wanwang.aliyun.com/