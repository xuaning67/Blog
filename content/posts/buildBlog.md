---
title: "BuildBlog"
date: 2022-08-01T14:08:53+08:00
lastmod: 2022-08-01T14:08:53+08:00
auther: lfl
# avatar：https://inews.gtimg.com/newsapp_bt/0/14662152563/1000
#authorlink: https://inews.gtimg.com/newsapp_bt/0/14662152563/1000
cover: https://img0.baidu.com/it/u=3379960287,3539585427&fm=253&fmt=auto&app=138&f=JPEG?w=889&h=500

categories:
  - 博客搭建
tags:
  - 学习
nolastmod: true
draft: false
---
搭建博客步骤

<!--more-->

写在开头。准确的说这应该算是第三篇博客，第一篇随机生成的，第二篇用来测试的，这一篇就用来介绍博客是怎么来的。
# 环境
+ Hugo
+ Git
+ Github
+ Netlify

# Hugo环境搭建
## 准备
Hugo下载地址：https://github.com/gohugoio/hugo/releases

Git下载地址：https://git-scm.com/
## Hugo安装
- 为了后续省事，直接下载了extended版本
- 新建Hugo文件夹，下层新建bin和Sites文件夹，将下载好的exe文件放到bin中
- 配置环境变量，在path下添加bin目录
## Git安装
下载，安装，完成
## 安装测试
cmd下，hugo version和git version测试安装状况
如果有必要，重启计算机。
# 本地搭建
## 初始化目录
- 进入Sites文件夹，右键执行git命令行
- 执行``hugo new site test``创建初始目录``test``任意取名。   
其中`content`目录为blog文件夹，`themes`为博客主题，`config.toml`为主题配置文件。
## 主题下载安装
主题网址: https://themes.gohugo.io   
根据个人喜好及需求随缘选择，因为有些主题需要用到个别环境   
根据主题内的要求进行下载  
为了主题的后续更新及优化，命令如下  

    cd test
    git init
    git submodule add https://github.com/g1eny0ung/hugo-theme-dream.git themes/dream
## 主题配置
编辑`config.toml`文件，一般在exampleSite文件夹中，将其移动到test目录下并根据个人需求进行修改。
## 查看本地博客
在test下打开命令行，执行`hugo server -D`，得到IP地址，在浏览器中打开即可。
## 新建博客
执行`hugo new posts/xxx.md`，xxx.md为采用markdown编辑的博客文件。撰写过程不赘述  
上述命令可用于再次创建自带模板的博客
# 上传Github
首先需要有个GitHub的账号，注册过程不再赘述...
其次，新建一个仓库，既然是博客，那么权限就设计为公开就好。
## SSH连接
为了方便后续的上传，使用SSH Key将Git和GitHub进行连接。
### 生成SSH密钥
命令行下执行
```
ssh-keygen -t rsa –C "youselfemail@xxx.com"（你自己的邮箱地址，推荐和注册github的邮箱保持一致）
```
执行完成后，在系统/用户//.ssh下会出现`id_rsa`私钥和`id_rsa.pub`公钥两个文件，复制公钥中的内容。  
在Github中进入`setting/SSH and GpG keys/new SSH keys`粘贴公钥内容，至此私钥和公钥匹配成功。保存。
## 上传博客
此处有一个问题，网上的教程上在使用Netlify部署时，跳过了hugo命令，上传博客是需要将public目录下的文件进行上传，而我在执行部署时，因为hugo命令和上传public文件命令部署失败，跳过了这两个步骤，所以需要编写完博客后手动build文件。
### 手动build（后续修改bug后再修改）
编写完博客后，在test目录下执行以下命令
```
Hugo
cd public
```
### 修改分支
Git的默认分支为master，Github的分支为main。所以将分支统一即可，下面的命令是将分支改为main。
```
git config --global init.defaultBranch main
```
### 合并README.md
新建Github仓库时初始化了一个README.md文件，需要先合并下来
```
git pull --rebase origin main
```
### 本地上传
```
git add .
git commit -m "上传博客文件"
```
### 远程上传
```
git push -u origin main
```
之后只需要输入git push也可。最后，刷新GitHub页面即可查看。   
后续再上传，可以从本地上传的步骤开始执行。
# Netlify部署

## 添加部署文件

原题主说是为了方便管理，在博客目录下整了一个netlify.toml文件，可没理解谁加载了这个配置文件。所以粘贴在下面，万一有用呢。

```toml
[build]
    publish = "public"
    command = "hugo --gc --minify"
[context.production.environment]
    HUGO_VERSION = "0.101.0"
    HUGO_ENV = "production"
    HUGO_ENABLEGITINFO = "true"
[context.split1]
    command = "hugo --gc --minify --enableGitInfo"
[context.split1.environment]
    HUGO_VERSION = "0.101.0"
    HUGO_ENV = "production"
[context.deploy-preview]
    command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"
[context.deploy-preview.environment]
    HUGO_VERSION = "0.101.0"
[context.branch-deploy]
    command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"
[context.branch-deploy.environment]
    HUGO_VERSION = "0.101.0"
[context.next.environment]
    HUGO_ENABLEGITINFO = "true"
```

## 注册netlify账号

Github绑定登录

## 绑定GitHub仓库

`New site from Git`->`GitHub授权`->`选择仓库`->`main分支`（按照自己的情况）->`buld command (hugo)`（按需自定，可省略，有啥用不知道）->`publish directory (publish)`（同上）->`deploy site`

分配域名后可修改二级域名

# 构建过程中出现的一些问题（尽力回忆）

