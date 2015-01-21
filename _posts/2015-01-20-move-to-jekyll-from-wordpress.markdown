---
author: jarry046
comments: true
date: 2015-01-20 16:42:57+00:00
layout: post
slug: '%e5%bb%ba%e7%ab%99%e6%88%90%e5%8a%9f%ef%bc%8c%e7%ba%aa%e5%bf%b5'
title: 在windows环境下从wordpress迁移到jekyll
intro: 迁移方法
wordpress_id: 4
categories:
- 代码之术
post_format:
- 日志
tags:
- ruby
- jekyll
- 博客
- git
---
使用了一段时间wordpress之后，感觉这个框架还是比较庞大和混乱，而且可能由于服务器的问题，有时会出现网站无法访问的情况。于是决定采用最近比较流行jekyll框架，将页面挂载到github上，由于本身框架就比较轻便以及github一直以来稳定优良的服务，速度和可靠性都比wordpress要好一点，而且接触了一下markdown这种编辑语言，发现既没有纯文本编辑的繁琐操作，又保证了良好的排版，写作的体验真是不能更好。

>在这里简单说一下，Jekyll是用Ruby编写的静态页面生成工具，比起wordpress因为不需要使用数据库，所以速度会快很多。而Github Pages原本是为了使项目更方便被人理解而提供的服务，现在除了做项目介绍之外，很多人也用它来挂载个人博客。

由于自己之前接触github的机会很少，所以git的操作也是找教程简单速成的，在这里简单记录一下在window环境下搭建jekyll并上传到github上的过程


## 1.安装ruby&Devkit


__方法1：直接安装Ruby&Devkit__

1. 从rubyinstaller上下载ruby和对应版本的Devkit，注意官方推荐最好选1.9.3的版本 <a href="http://rubyinstaller.org/downloads/">:http://rubyinstaller.org/downloads/</a>
2. 安装ruby
3. 将Ruby的安装路径如  C:\Ruby200-x64 添加到系统环境变量
4. 打开命令行输入	 ruby -v 检查是否安装成功
5. 运行Devkit的安装包，选择解压到一个文件夹下面（DevKit 是一个在 Windows 上帮助简化安装及使用 Ruby C/C++ 扩展如 RDiscount 和 RedCloth 的工具箱。）
6. 打开刚才解压了Devkit的文件夹下面的 config.yml,在文件的末尾添加 `- 你的Ruby安装路径（如C:\Ruby200-x64）`
7. 回到命令行窗口，输入如下语句  
	 `ruby dk.rb review`  
	`ruby dk.rb install`

__方法2：通过railsInstaller安装__

用easyInstall是比较讨巧的办法，它会把ruby、rails、Devkit以及Bundler、Git等等同时打包安装好，如果同时也有在windows上开发ruby on rails的需求的话，用easyInstall安装简直是独一无二的选择（本人亲测手动在windows上安装新版本的rails会出各种bug几乎没办法成功，血淋淋的教训),一键安装无需教程

下载地址：

[http://www.railsinstaller.org/en](http://www.railsinstaller.org/en)

##2.安装Jekyll

1. 打开命令行，输入`gem install jekyll`
2. 输入 `jekyll -v` 如果有版本信息提示说明安装成功

##3.启动Jekyll

1. 选择一个文件夹如D:/blog作为工程文件夹
2. 在该工程文件夹下打开命令行工具
3. 在命令行中输入 `jekyll new blog` （blog就是工程名，可以任意）
4. 在命令行中输入 `cd blog` 进入工程文件夹下（blog为刚才的工程名）
5.  在命令上中输入 `jekll server` ，就可以在浏览器上通过 `localhost:4000`来访问你的博客啦

##4.从wordpress迁移文章

从wordpress迁移到jekyll的原理是将wordpress的日志转成jekyll所需的markdown格式的文件 ，这里用到的工具需要__安装Python__，从度娘那里找到一个安装Python的教程
[http://www.cnblogs.com/hongten/p/hongten_python_install.html](http://www.cnblogs.com/hongten/p/hongten_python_install.html) 

1. __将wordpress内容在后台导出成xml文件__，在仪表盘里面选__工具-导出__，下载导出的文件即可
2. 下载或者用git clone [https://github.com/thomasf/exitwp](https://github.com/thomasf/exitwp) 里面的工程文件
3. 将刚才下载的文件解压到一个文件夹下
4. 进入该文件夹，把刚才下载的xml文件复制到 wordpress-xml文件夹下
5. 进入命令行，输入 `python exitwp.py` 执行，这里可能会报错说没有找到bs4这个模块，需要安装BeautifulSoup4这个依赖包，步骤如下

	1. 到网站上下载：[http://www.crummy.com/software/BeautifulSoup/bs4/download/](http://www.crummy.com/software/BeautifulSoup/bs4/download/)
	2. 解压文件到C:\Python27
	3. cmd运行C:\Python27\BeautifulSoup>python setup.py install
	4. 测试一下是否能导入 `>>>ipmort bs4`
6. 运行完成后在build文件夹下找到`\_posts`文件夹，将里面的内容全部复制到blog工程文件下的_posts文件夹里面
7. 在blog工程文件夹下打开命令行，输入`jekyll b` 重建一下工程，再访问`localhost:4000`就可以看到wordpress里面的文章被全部转移到jekyll里面啦  

__注意__  

在这里可能会遇到一个错误，如果你的category是带有中文名的话jekyll是不支持的，会提示`"\xAF\xBB" from GBK to UTF-8`这样的错误，这时候就需要改变一下jekyll的文件组织方式。避开的方法，在_config.yml中新增一行
		
		permalink: /:year/:month/:day/:title  

##5.上传工程到github，生成静态页面

###Git环境配置

1. 在[github](https://github.com/)上申请一个账号，记得要记住自己的用户名哦，注意要验证一下邮箱，否则没办法生成github pages
2. 安装msysgit，在[http://msysgit.github.io/]( http://msysgit.github.io/)上下载，msysgit是windows版的Git，安装完成之后在文件夹下点鼠标右键就会有git的相关工具了
3. 在blog文件夹下面点击右键，进入Git Bash 
4. 输入命令 `$ ssh-keygen -t rsa -C "你刚才注册的邮箱地址@xxx.com"` ,回车回车回车，生成ssh key
5. 进入~/.ssh文件夹，将`id_rsa.pub`文件的内容全部复制
6. 在Github的主页左上方上点Account Settings按钮，选择SSH Keys项，把复制的内容粘贴进去，点击 Add Key
7. 回到Bash，输入 `$ssh -T git@github.com`看是否设置成功
8. 设置账号信息，在Bash里输入  
	 `$ git config --global user.name "你的名字"`  
	`$ git config --global user.email "your_email@youremail.com"`  
  
###上传Github Pages
1. 在Github里面建立一个仓库，名为`username.github.io`，username为你的用户名
2. 进入blog项目文件夹下，右键进入Git Bash
3. 输入以下命令  
	`git init`   
	`git remote add origin git@github.com:你的用户名/你的用户名.github.io.git`  
	`git add.`  
	`git commit -m "first commit"`  
	`git push origin master`
4. 在github上查看一下是否push成功
5. 通过浏览器访问username.github.io，就可以看到效果啦，如果10分钟内还是404的话查看一下自己的邮箱，看是否是因为没有通过验证导致的失败






   




		

