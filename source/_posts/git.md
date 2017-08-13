---
title: git常用命令
date: 2016-06-15 20:54:34
tags:
---
# 使用方法

安装 Git 之后，你要做的第一件事情就是去配置你的名字和邮箱，因为每一次提交都需要这些信息：

git  config  --global   user.name   "name"
git  config  --global   user.email   "email@qq.com"
# 指令说明

git  status               查看工作区所有文件状态
git  add  1.jpg          把指定文件添加到暂存区
git  add  *.jpg          把指定后缀名的文件添加到暂存区
git  add  *              把所有文件添加到暂存区
git  add  .              把所有文件添加到暂存区
git  commit  -m  "消息内容"   把暂存区的修改提交给仓库
git  log                 查看仓库中所有的提交记录

git  reset  --hard  版本号     工作区回退到指定的版本
git  log       查看当前已经生效的所有版本号
git  reflog     查看当前已经生效&撤销生效的所有版本号
# 远程仓库相关

git  clone  远程仓库的地址
git  clone  git@github.com:jquery/jquery.git
git  clone  https://github.com/jquery/jquery.git
# 创建关联使用方法

git  init           本地创建一个空白仓库
git  remote       查看当前仓库关联到哪些远程仓库
git  remote  add  关联仓库名  远程仓库的地址
git  pull  关联仓库名  master    从远程仓库“拉取(pull)”已有的文件和版本信息到本地仓库
git  add  .        修改本地仓库中的文件，添加到本地暂存区，提交给本地的仓库
git  commit   -m   "说明信息"
git  push  远程仓库名  master    把本地仓库中的新的版本信息“推送(push)”给远程仓库

git subtree push --prefix=dist origin gh-pages  建立分支结构