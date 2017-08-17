---
layout: page
title: ""
permalink: /resume/
---

<h1 style="text-align:center;">个人简历</h1>

<br/>

## 孙福生

| :--- | :--- |
| Phone          | 156 9821 9533          |
| Email          | sfsheng0322@126.com          |
| GitHub          | [https://github.com/sfsheng0322](https://github.com/sfsheng0322)          |
| Blog          | [http://sunfusheng.com](http://sunfusheng.com/)          |

<br/>

## 专业技能

● GitHub上开源 [StickyHeaderListView](https://github.com/sfsheng0322/StickyHeaderListView) 项目，模仿百度外卖首页同时用于回家吃饭首页  
● GitHub上开源 [MarqueeView](https://github.com/sfsheng0322/MarqueeView) 自定义View，用于循环显示垂直翻页公告  
● GitHub上开源 [Bingo](https://github.com/sfsheng0322/Bingo) 项目，用于收集和阅读优秀的技术文章  
● 热爱技术分享，空闲时间在 [简书](http://www.jianshu.com/users/88509e7e2ed1/latest_articles)、[个人博客](http://sunfusheng.com/)、[新浪微博](http://weibo.com/u/3852192525)、掘金、开发者头条上分享技术文章  
● 熟练掌握git相关操作  
● 熟练掌握基本控件、自定义控件的使用  
● 使用Android Studio开发，掌握Gradle构件项目  
● 熟悉安卓应用框架设计，熟悉MVC和MVP架构模式    
● 熟悉Handler消息通信机制，研究并使用EventBus3.0  
● 掌握View树的绘制流程和事件分发机制  
● 熟悉线程和线程池的使用，熟悉进程间通信  

<br/>

## 工作经验

### 2015年11月至今，回家吃饭，高级Android工程师
 
[【回家吃饭】](http://www.jiashuangkuaizi.com/)是一个基于地理位置、共享身边美食的全国最大的家庭厨房共享平台，致力于挖掘厨艺达人，以配送、上门就餐、自取多种方式为忙碌的上班族、不愿下厨的年轻人提供安心可口的家常菜，致力于解决对健康美食有需求与富余生产力衔接的问题，创造一种全新的生活方式。

安卓用户端两人开发，[工作期间主要完成如下功能](http://www.jianshu.com/p/329312a93266)：   
一、回家吃饭项目从eclipse上转到studio环境上开发  
二、项目管理从svn迁移到git上，多分支管理开发  
三、网络库从xUtils更换为OkHttp3.2  
四、使用动态代理AOP编程框架统一拦截实现网络回调  
五、首页全新改版，优化定位功能  
六、重构厨房详情页  
七、重写购物车，自定义ShoppingView  
八、优化图片加载、内存泄漏等  

<br/>

### 2014年10月至2015年11月，创新工场，Android工程师

[【美丽屋】](http://bj.meiliwu.com/)是一款可以根据城市或者地图搜索二手房、待出租房，浏览房屋相关信息，具有IM聊天功能可以与卖房者、租房者沟通，同时该应用还拥有贷款计算器和税费计算器功能。

在此期间我主要完成该应用的注册、登录和引导页等基本功能，实现租房详情页，使用百度地图实现地图查找房源功能，实现税费计算器和贷款计算器功能等。

<br/>

###  2013年2月至2014年9月，卡尔电气，Android工程师(独立开发)

[【视界通】](http://www.kaer.cn/pro-836.html)是以点对点信息服务为基础(P2P)的视频监控项目。具体功能为实时视频查看、历史录像和远程录像回放、云台设备控制、语音对讲和视频抓拍等功能。主要使用ZMQ网络通信技术传输音视频数据，使用JNI调用C语言实现的H264解码库解析终端设备发送的视频和音频数据。
 
[【蓝牙智能固话】](http://www.kaer.cn/pro-834.html)是一款基于4G网络或有线电话网络具有蓝牙通话功能的智能电话机。用户可以通过手机蓝牙控制带有蓝牙模块的智能电话机拨打企业通讯录中的号码、接听来电，查看通话记录等功能，同时话机也可以控制手机拨打电话、挂断电话。主要使用的技术包括：Android下蓝牙的连接、重连和数据传输、通过WebService与服务器交互以获取企业通讯录数据、使用SQlite数据库保存企业通讯录数据等。

<br/>

## 个人开源项目

#### 1、[【StickyHeaderListView】](https://github.com/sfsheng0322/StickyHeaderListView)

StickyHeaderListView是基于实际需求做出的灵活可变的UI视图，具体实现了如下功能：  
一、支持下拉刷新和上拉加载更多功能。  
二、支持无限循环的广告位。  
三、使用GridView实现可动态配置的频道位、运营位和分割线。  
四、自定义FilterView实现筛选功能，同时支持动画显示与动画隐藏。  
五、支持FilterView滑动到顶部后吸附悬浮。  
六、支持标题栏背景颜色渐变和字体颜色渐变。  
七、实现了数据不足一屏动态添加空数据占位。  
八、数据为空时ListView多type加载暂无数据视图。  

Github地址：[https://github.com/sfsheng0322/StickyHeaderListView](https://github.com/sfsheng0322/StickyHeaderListView)

APK下载地址：[http://fir.im/StickyListView](http://fir.im/StickyListView)

#### 2、[【MarqueeView】](https://github.com/sfsheng0322/MarqueeView)

垂直翻页公告，可以将一大段文字根据屏幕显示的宽度截取，不断滚动显示，该项目也可以叫做垂直的跑马灯。

Github地址：[https://github.com/sfsheng0322/MarqueeView](https://github.com/sfsheng0322/MarqueeView)

APK下载地址：[http://fir.im/MarqueeView](http://fir.im/MarqueeView)

<br/>

## 教育背景

2009年9月至2013年6月，山东工商学院，本科，自动化专业  
英语CET4  
全国大学生电子设计大赛优秀奖  
校智能车比赛二等奖  
有嵌入式软件开发和PCB绘制基础  

<br/>

## 兴趣爱好

喜欢自己动手写小应用程序  
爱运动，爱旅行  
工作踏实认真，有钻研精神，接受新知识能力强  






