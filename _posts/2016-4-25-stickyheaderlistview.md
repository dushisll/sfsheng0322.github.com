---
layout: post
author: 孙福生
title: Android 开源之StickyHeaderListView 标题渐变、吸附悬停、筛选分类、动态头部
cover: "zzz"
categories: 项目
tags: Technology
---

[StickyHeaderListView](https://github.com/sfsheng0322/StickyHeaderListView) 是基于实际需求做出的灵活可定制的UI功能，具体实现功能如下：  
一、支持无限循环的广告位。  
二、高度可动态配置的Header2和Header3（使用GridView实现）。  
三、主要功能：分类、排序和筛选布局滑动到顶部后吸附、悬停。  
四、自定义FilterView筛选控件，支持动画显示与动画隐藏。  
五、支持标题栏背景颜色渐变、字体颜色渐变。  
六、数据不足一屏动态添加空数据占位。  
七、数据为空时，ListView加载暂无数据视图。  
八、思路清晰、界面优美，添加ripple点击效果。  
九、支持下拉刷新和上拉加载更多功能。

### 动态效果图：

<img src="/assets/gifs/stickyheader.gif" style="width: 50%;"/>

<img src="/assets/gifs/stickyheader2.gif" style="width: 50%;"/>

### [GitHub开源地址](https://github.com/sfsheng0322/StickyHeaderListView)

### [APK下载地址](http://fir.im/StickyListView)

### 实现思路

StickyHeaderListView 主要是通过 ListView 添加头部实现，将复杂的头部分解为若干部分，如下图：Header 1(广告位)、Header 2(频道位)、Header 3(运营位)、Header 4(分割线) 和 Header 5(筛选头部)，这样各个Header部分的UI和逻辑可以单独拿出去处理，具体可以参考我的[开源代码](https://github.com/sfsheng0322/StickyHeaderListView)。

<img src="/assets/android/StickyHeaderListView_sumary.png" style="width: 50%;"/>

Header 1: 它的高度影响标题栏的颜色渐变。 
 
Header 2: 使用GridView实现，自定义[FixedGridView](https://github.com/sfsheng0322/StickyHeaderListView/blob/master/app/src/main/java/com/sunfusheng/StickyHeaderListView/view/FixedGridView.java)，高度不受ListView的影响，一行显示几个自己可以根据需求设置。
  
Header 3: 和Header 2一样的实现方式，要注意的地方就是分割线的设置，我实现的思路是设置GridView的背景颜色的分割线的颜色，再设置如下的四个属性：paddingTop、paddingBottom、horizontalSpacing、verticalSpacing为1px，这样分割线就均等了。

	android:background="@color/font_black_5"
	android:paddingTop="1px"
    android:paddingBottom="1px"
    android:horizontalSpacing="1px"
    android:verticalSpacing="1px"
    
Header 4: 这个头部布局是需求上的，UI加上整体更加好看，为什么我要单独拿出来，主要考虑到以下的原因：如果让Header 5达到吸附悬停的效果，需要知道Header 5到顶部的距离，如果把分割线加到Header 5上，那在移动的时候还需要减去这个高度；而如果加到Header 3上，Header 3是服务器动态配置的，如果没有Header 3的头部怎么办，那就加到Header 2上等，这样逻辑就比较麻烦，干脆我直接单独拿出来，作为一个头部布局动态添加。

Header 5: 这个筛选头部是个假的布局，主要处理未吸附悬停时的点击事件，点击之后滑动到顶部这时顶部的隐藏的筛选布局显示出来达到吸附悬停的效果。同时我将这个筛选布局定义一个 [FilterView](https://github.com/sfsheng0322/StickyHeaderListView/blob/master/app%2Fsrc%2Fmain%2Fjava%2Fcom%2Fsunfusheng%2FStickyHeaderListView%2Fview%2FFilterView.java)，将分类、排序和筛选的UI处理和逻辑封装起来，方便其它页面的二次使用。

#### 还有两点需要特别注意：  
一、如果数据不满一屏，比如就一条数据，那点击筛选它是没办法滑动到顶部的，因为她的高度不够，我的解决方法是添加若干个空数据，空数据的size是根据实际一屏要显示的个数减去现在的个数，这样可以达到整体可以滑动的高度，参考 [TravelingAdapter](https://github.com/sfsheng0322/StickyHeaderListView/blob/master/app%2Fsrc%2Fmain%2Fjava%2Fcom%2Fsunfusheng%2FStickyHeaderListView%2Fadapter%2FTravelingAdapter.java) 文件。

二、如果数据为空时并且我还需要无数据的占位图，如果在 ListView 底部加上无数据的布局这样的效果是不好的，所以我还在这个Adapter上做文章，让它加载一个无数据的视图布局，同样参考 [TravelingAdapter](https://github.com/sfsheng0322/StickyHeaderListView/blob/master/app%2Fsrc%2Fmain%2Fjava%2Fcom%2Fsunfusheng%2FStickyHeaderListView%2Fadapter%2FTravelingAdapter.java) 文件，每一个Item的高度： height ＝ 屏幕的高度 － 标题栏高度 － 筛选View高度，这样设置一个这样的高度的Adapter，再 notifyDataSetChanged() 一下，整体的视图不会变化，无数据的占位图也自然而然的显示了。

### 最后

具体实现代码移步 [GitHub](https://github.com/sfsheng0322/StickyHeaderListView)，下载 [APK](http://fir.im/StickyListView) 体验，感谢你的关注，欢迎star，希望对你有帮助，如遇到问题请联系我，最后再贴几张截图方便你查看。

#### 滑动到一半时标题栏渐变

<img src="/assets/android/StickyHeaderListView2.png" style="width: 50%;"/>

#### 滑动到顶部，FilterView 吸附悬停

<img src="/assets/android/StickyHeaderListView3.png" style="width: 50%;"/>

#### FilterView 动画显示与隐藏

<img src="/assets/android/StickyHeaderListView4.png" style="width: 50%;"/>

#### 数据为空时的占位图

<img src="/assets/android/StickyHeaderListView5.png" style="width: 50%;"/>
    
