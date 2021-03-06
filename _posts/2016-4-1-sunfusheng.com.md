---
layout: post
author: 孙福生
title: 依托于Github的个人博客自定义域名设置
cover: "zzz"
categories: 工具
tags: Technology
---
   
每一天都是特别的，但四月一号的今天尤其Special，不仅仅是因为愚人节，因为从今天起原来的个人博客域名[sfsheng0322.github.io](http://sfsheng0322.github.io/)又多了个兄弟新的简洁域名[sunfusheng.com](http://sunfusheng.com/)，当然两个域名都可用，前者在通过Jekyll搭建博客后默认的域名，新域名则是从阿里云那买的；但是我所有的博客文章都在GitHub上啊，难道还要重新部署到阿里云上嘛，当然不用啦，下面我记录了依托于Github的个人博客自定义域名设置。

先看看顶级国际域名证书，逼格还是蛮高的嘛！

<img src="/assets/sunfusheng.com_cert.jpg" style="width: 50%;">

买完域名后，怎么搞，第一次搞，我也不知道怎么搞，但目的很简单就是域名绑定，域名解析到我原来的地址上

<img src="/assets/cname_icon1.png" style="width: 80%;">

百度一下，你就知道；拉到吧，百度真不好找，Google一下吧，哦哦，来了，上步骤：

#### 首先  
在博客的根目录下面，新建一个名为CNAME的文本文件，CNAME文件没有后缀，里面写入你要绑定的域名，比如sunfusheng.com。  

如果绑定的是顶级域名，则DNS要新建一条A记录，指向 103.245.222.133。如果绑定的是二级域名，则DNS要新建一条CNAME记录，指向 username.github.com（请将username换成你的用户名）。

#### 其次  
将_config.yml文件中的baseurl改成根目录”/”。
 
#### 最后  
将设置买的域名，我的域名设置如下：

<img src="/assets/cname_icon2.png" style="width: 80%;">


这当中有几个新概念，可能你不知道，我帮咱俩查了下：

##### 域名解析  
	域名解析就是域名到IP地址的转换过程。IP地址是网路上标识您站点的数字地址，为了简单好记，
	采用域名来代替ip地址标识站点地址。域名的解析工作由DNS服务器完成。

##### A记录  
	A记录是用来指定主机名（或域名）对应的IP地址记录。用户可以将该域名下的网站服务器指向到自己的web server上。
	同时也可以设置您域名的二级域名。

##### CNAME记录  
	CNAME记录，即：别名记录。这种记录允许您将多个名字映射到同一台计算机。 通常用于同时提供WWW和MAIL服务的计算机。
	例如，有一台计算机名为“host.mydomain.com”（A记录）。 它同时提供WWW和MAIL服务，为了便于用户访问服务。可以为该计算机设置两个别名（CNAME）：WWW和MAIL。

##### TTL值  
	即 Time To Live，缓存的生存时间。指地方dns缓存您域名记录信息的时间，缓存失效后会再次到DNSPod获取记录值。
	600（10分钟）：建议正常情况下使用 600。

	60（1分钟）：如果您经常修改IP，修改记录一分钟即可生效。长期使用 60，解析速度会略受影响。

	3600（1小时）：如果您IP极少变动（一年几次），建议选择 3600，解析速度快。如果要修改IP，提前一天改为 60，即可快速生效。

	一开始测试的时候可以把TTL的时间先改小些，确认没问题了再改大。

##### 另外，我A纪录类型中的纪录值IP是通过下面的方式取得：

	windows下按Win+R，输入cmd后回车，在命令行里输入ping username.github.io（请将username换成你的用户名），
	回车后会看到一个IP，就是它了。

	如果是mac，可以在终端输入dig username.github.io（请将username换成你的用户名），同样回车后会看到一个对应的IP地址。




