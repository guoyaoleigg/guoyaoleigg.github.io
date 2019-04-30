---
title: 安装hexo
date: 2019-02-24 15:44:54
tags: 技术分享
---

### 1 下载 node.js 安装
安装完成后在命令行窗口（最好是root用户）下
```shell
node -v
npm -v
```
### 2 安裝cnpm
```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org

 cnpm -v 
 #查看安装好的版本
```
### 3安装 hexo
```shell
 cnpm install -g hexo-cli
 
 hexo -v
# 查看版本
```
### 4初始化hexo
```shell
 mkdir blog
 #新建后进入blog文件夹
hexo init
#初始化 hexo博客
 
```
### 5启动hexo
```shell
hexo s
#启动博客 在本地测试博客用

hexo n
#新建一个博客 文件在C:\WINDOWS\system32\blog\source\_posts

hexo clean
#清除缓存文件 (db.json) 和已生成的静态文件 (public)。在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

hexo g
#生成静态文件
```
### 6在github上部署
```shell
cnpm install --save hexo-deployer-git
#安装 git
```
在github上新建一个库，命名要和自己的昵称一致
guoyaoleigg.github.io

修改 _config.yml 文件 在最下面deploy中添加
```yml
deploy: 
  type: git
  repo: https://github.com/guoyaoleigg/guoyaoleigg.github.io.git
  branch: master
```

向github推送代码
```shell
hexo d
```
### 7访问 guoyaoleigg.github.io 完成
### 8更换主题
1.首先找到自己喜欢的主题
2.克隆主题到本地

```shell
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```
3.修改 _config.yml 文件
```yml
theme: yilia
```

4.推送代码
```shell
hexo clean
hexo g
hexo s
hexo d 
#把代码推送到github
```