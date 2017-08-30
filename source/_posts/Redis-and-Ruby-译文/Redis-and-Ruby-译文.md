---
layout: post
title: Redis and Ruby (译文)
date: 2011-09-15 12:37:06
tags:
---

# Redis and Ruby (译文)


_原文地址：[http://www.programmersparadox.com/2009/06/02/redis-and-ruby/](http://www.programmersparadox.com/2009/06/02/redis-and-ruby/)_

在五月份的一次 raleigh.rb 聚会中， Kevin Smith进行了一次关于多形式数据库的演讲，其中包括著名的 CouchDB 和 Redis。

这一次的演讲让我确信Redis将会是支持 getencouraged（一个小型的类似twitter的项目）最好的数据库系统了。

Redis是一种加速的key-value数据库，特别适合这种只需要保存tweets的小型twitter项目。作为Ruby世界的新鲜事物，我之前没有遇到过这样的阻碍让我如此沮丧，而这些问题同样也会摆在其他人面前。其中一个障碍是我对ruby中gem的系统机制理解不够深入，而且找不到一个关于Redis的gem能够让我轻松的gem install去使用。这意味着我必须知道如何用代码从github上安装Redis ruby的客户端开发包到本地。想到读者们也许也会像我这样对它们犯迷糊，我在这里列举出使用redis-rb客户端开发包来安装Redis和redis gem到本地系统的步骤。  
第一步，从github上下载redis-rb客户端开发包，地址如下：  
[http://github.com/ezmobius/redis-rb/tree/master](http://github.com/ezmobius/redis-rb/tree/master)

网上也有其他的Redis Ruby客户端开发包，但由于我之前用过redis-rb，所以我推荐使用它。为什么不直接从Redis开始，而是从一个Redis的客户端开始呢？因为redis-rb提供了rake任务已经可以帮你安装Redis了。当然你也可以不用rake任务来安装Redis。

一旦你下载了redis-rb，把它解压好了，然后在你的终端上键入cd 进入目录。如果你的系统还未安装rspec gem，请安装它：

`sudo gem install rspec`

现在执行rake的命令安装Redis：

`rake redis:install`

执行上面的命令会将Redis安装到/usr/bin目录。Redis的配置文件redis.conf位于/etc目录。你能够通过执行 /usr/bin/redis-server 来启动Redis服务器。因为Redis会提供默认的配置文件，所以通常情况下不需要设置它；不过推荐去打开这个文件看看，它里面有许多值得探究的地方。  
接下来，redis-rb需要你安装dtach，执行下面命令：

`rake dtach:install`

键入以上命令将dtach安装到 /usr/bin/目录下。除此之外你不必关心dtach了，只需要保证它已经在系统里面，以便redis-rb能够找到它。

最后一个步骤是安装redis-rb gem，让它能在你的代码中使用。这一步很简单：

`rake gem`

这个命令目的是把redis-rb gem打包放在当前文件夹下面。如果你已经进入了这个打包后的目录，你就可以安装这个gem了：

`sudo gem install redis`

_注意：虽然在github的repo上是叫做redis-rb,但repo创建的gem叫做redis。_

这样，你不仅把Redis安装到你的系统，而且也能够通过redis gem利用ruby代码连接Redis了。这里有几个例子帮你把程序跑起来（这里的例子是基于github上的redis-rb repo的，你也可以在这个项目里找到更多的例子）：

把代码保存到文件里面，然后在你的机子上开启Redis（/usr/bin/redis-server），执行代码，见证它的魔术戏法吧。希望能够帮助你开启Redis之旅，Enjoy！
