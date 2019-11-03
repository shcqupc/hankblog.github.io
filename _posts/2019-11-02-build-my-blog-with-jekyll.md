---
layout: post
title:  "Build my blog with Jekyll"
date:   2019-11-02 17:25
categories: jekyll
tags: jekyll RubyGems
---

* content
{:toc}
# 搭建过程
## 1. 获取Template
我先是找到一个样版[blog](https://643435675.github.io/)，根据地址找到其GitHub上对应的[库](https://github.com/643435675/643435675.github.io)，将其代码下载下来(Download ZIP)， 解压为名为**xudailong.github.io-master**文件夹





## 2. 搭建本地环境
参考在jekyll的官网[(http://jekyllrb.com)](https://jekyllrb.com/docs/) 上的流程，主要环节有：安装Ruby，安装RubyGems，安装jekyll，安装代码高亮插件，安装node.js

### 安装Ruby

ruby官网下载安装： [https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)

安装完成后命令行运行下面代码能得到版本号即按照成功：  
`ruby -v`
 

### 安装RubyGems

官网下载ZIP版本 [https://rubygems.org/pages/download](http://rubygems.org/pages/download) 

cd到RubyGems目录，执行安装：  
`ruby setup.rb`

### 安装GCC和Make
到[官网](https://osdn.net/projects/mingw/releases/)下载并安装**mingw-get-setup.exe**  
安装完成后启动MingGW Installation Manager，安装列表中的mingw32-gcc\*和mingw32-make\*的package  
运行代码检查是否安装成功：
```
gcc -v
g++ -v
make -v
```

### 用RubyGems安装Jekyll

执行下面的语句安装：  
```
gem install jekyll
gem install jekyll bundler
```

### 创建博客

在e盘根目录下执行下面代码创建新的工作区   
`jekyll new shcqupc.github.io-master`

然后将之前得到的样版**xudailong.github.io-master**文件夹里的内容复制到**shcqupc.github.io-master**文件夹里，cd到blog文件夹，运行下面代码开启服务器   
`e:\shcqupc.github.io-master>bundle exec jekyll s --trace`

### Github page托管
在github上创建blog repository,可以参考[https://pages.github.com/](https://pages.github.com/)
先将本地**shcqupc.github.io-master**文件夹的内容剪切出来备份，运行代码创建本地git目录，清空目录，再将备份的内容复制回去，并PUSH到github
`git clone https://github.com/shcqupc/shcqupc.github.io shcqupc.github.io-master`

## 后续

*  整个安装过程参考了jekyll官网，和该篇[博文](https://643435675.github.io/2015/02/15/create-my-blog-with-jekyll/)  
*  遇到了很多坑，主要是本地环境问题
*  需要完善站内的一些链接
*  需要添加访问量统计

---

## 可能出现的问题
- **在启动服务时出现：**  
<font color="#FF4500"> Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/! </font>

- **Solution:**   
_config.yml文件中添加：  
gems: [jekyll-paginate] paginate: 5   
并在Gemfile中添加:  
gem 'jekyll-paginate', group: :jekyll_plugins  

&nbsp;

---







