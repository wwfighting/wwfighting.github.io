---
layout: post
title:  "Github pages + Jekyll + Markdown 搭博客"
date:   2016-02-16 15:14:54
categories: 其他
comments: true
---

* content
{:toc}

第一篇博客就用来讲解一下现在这个博客的搭建过程，摸索了三天，终于弄懂了其过程。并搭建成功！该博客利用 github pages + jekyll 搭建，并利用markdown进行正文的编写（以下均基于Windows平台）。

## 安装工具

### 安装Git

Git是一个分布式版本控制系统，用来编译ruby，测试jekyll以及安装相关的工具，下载[Git](http://git-scm.com/download/)。

### 安装Ruby

jekyll是基于Ruby开发的，所以想要在本地构建一个测试环境，肯定需要ruby的开发和运行环境，windows下可以使用[Rubyinstaller](http://rubyinstaller.org/downloads/)安装，安装的时候直接点击下一步就行了。

### 安装RubyDevKit

点击下载[DevKit](http://rubyinstaller.org/downloads/)，注意版本要与之前的Ruby安装一致，安装好之后记住其安装路径，然后打开git,
先切到该路径，执行如下指令:

	$cd .../yoursite/
	$ruby dk.rb init

然后执行如下指令:

	$ruby setup.rb

如果没有setup.rb，执行：

	$ruby dk.rb install

### 安装Rubygems

目前最新版的Ruby已经自带了Gems，所以找到刚刚安装的Rubyinstaller的路径,然后再cd到该gem的路径下，然后执行如下指令就行:

	$cd ..../gems/
	$ruby setup.rb

因为天朝的网络问题，很多外国的软件以及资料等等都需要翻墙下载，如果没有vpn，很多都使用镜像网站，ruby的国内仓库镜像网站很多
都用的淘宝上的一个[ruby配置]http://ruby.taobao.org/，里面配置很全。指令如下先删除本身_config.yml地址，保证只有一个地址，
然后选择该镜像地址即可：

	$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
	$ gem sources -l

## 安装jekyll

有了之前的准备工具之后，安装jekyll就简单多了，还是先打开git，然后执行如下指令:

	$gem install jekyll

jekyll依赖的组件就会自动下载


### 使用现成的jekyll theme模板

因为博客使用[jekyll](http://jekyll.bootcss.com/)搭建的，对于新手来说（尤其是没有什么前端经验的人，比如我），最好是使用模板
比较好，直接用现成的的[jekyll theme template](http://jekyllthemes.org/)，选择一款你喜欢的模板，可以用git进行clone到本地，
也可以直接download下来，然后解压到一个指定目录下。打开git，执行如下指令:

	$cd .../your jekyll theme template/
	$jekyll serve

然后就会启动jekyll服务，jekyll引擎会在该目录下自动生成一个文件夹_site，打开浏览器，输入“ip地址输:4000",https://your ip:4000
端口号为4000，就能看到在本地运行的博客demo了。

## 安装GitHub Desktop

* 先下载安装[github desktop](https://desktop.github.com/)，然后在客户端登陆你的个人GitHub账户。登陆之后，可以先在本地
create一个repository，repository的命名规范为yourname.gitHub.io
![创建repo](https://raw.githubusercontent.com/wwfighting/wwfighting.github.io/master/_posts/github_repo_create.png)

* 再将刚刚下载好的jekyll模板里的源文件全部copy到yourname.github.io文件夹下，注意别把_site文件夹copy进去！因为GitHub会根据源
码自动创建_site文件夹里的内容。

* copy好之后，通过客户端commit然后push到github，然后sync就同步到了github网页端了。然后在浏览器输入 yourname.github.io 即可访
问到自己的博客了。

* 最后再修改源码，删除原来作者的一些文章，自己进行创作。可以先在本地通过运行jekyll serve然后测试一下，达到预期效果，便可以push到
自己的博客上。

## 使用Markdown编写博客

下载并安装一个markdown IDE [Atom](https://atom.io/)，然后就可以对_post文件夹下的.md文件操作，markdown的语法很简单，用来写
博客非常适合不过了，只需要查看[markdown文档](http://www.appinn.com/markdown/)或者对着源码敲一遍就能学会，目测10分钟学会。



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
