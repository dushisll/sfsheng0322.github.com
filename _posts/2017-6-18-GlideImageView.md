---
layout: post
author: 孙福生
title: 基于Glide V4.0封装的GlideImageView，可监听加载图片时的进度
cover: "zzz"
categories: 项目
tags: Technology
---

<img src="http://ourv0v3ed.bkt.clouddn.com/GlideImageView1.gif">

### GlideImageView 是基于[Glide](https://github.com/bumptech/glide) V4.0设计的，实现如下特性:  
1、通过提供的属性可以设置图片的圆角、边框。  
2、可以设置点击触摸图片时的颜色、透明度。  
3、一行代码加载来自网络、res、SDCard中的图片，可加载成圆形。  
4、可以监听加载图片时的进度。

<br/>

#### 设置圆角、边框的图片，触摸图片时的效果，显示Gif图的效果

<table> <tr> <td><img src="/assets/GlideImageView/image4.png"></td> <td><img src="/assets/GlideImageView/gif6.gif"></td> <td><img src="/assets/GlideImageView/gif1.gif"></td> </tr></table>

<br/>

### [GitHub地址](https://github.com/sfsheng0322/GlideImageView)

### [APK下载地址](https://fir.im/GlideImageView)，去手机上体验吧 (◐‿◑)

<br/>

### 具体使用说明如下

#### Gradle:

    compile 'com.sunfusheng:glideimageview:1.0.0'
   
#### Maven:

    <dependency>
      <groupId>com.sunfusheng</groupId>
      <artifactId>glideimageview</artifactId>
      <version>1.0.0</version>
      <type>pom</type>
    </dependency>

<br/>

#### ShapeImageView 和 GlideImageView 共同的属性

该库提供了一个[ShapeImageView](https://github.com/sfsheng0322/GlideImageView/blob/master/GlideImageView/src/main/java/com/sunfusheng/glideimageview/ShapeImageView.java)类，可以在xml当中，也可以在代码中设置图片的一些属性，
当然这些属性也可以在[GlideImageView](https://github.com/sfsheng0322/GlideImageView/blob/master/GlideImageView/src/main/java/com/sunfusheng/glideimageview/GlideImageView.java)上面设置，具体属性如下

| Attribute 属性 | Description 描述 | 
| :--- | :--- | 
| siv_border_color | 边框颜色 | 
| siv_border_width | 边框宽度 | 
| siv_pressed_color | 触摸图片时的颜色 | 
| siv_pressed_alpha | 触摸图片时的颜色透明度: 0.0f - 1.0f | 
| siv_radius | 圆角弧度 | 
| siv_shape_type | 两种形状类型：默认是0:rectangle、1:circle | 

#### 下面是在xml中和代码中设置的效果

| xml中设置 | 代码中设置 | 
| :--- | :--- | 
| ![](https://user-gold-cdn.xitu.io/2017/6/19/28a1f76d9474803f7dfbe0e132c6dca8) | ![](https://user-gold-cdn.xitu.io/2017/6/19/caefe3b2089b4e41db5e751b4ca841e3) |

<br/>

#### 一行代码加载来自网络、res、SDCard中图片

    public GlideImageView loadImage(String url, int placeholderResId);
    public GlideImageView loadLocalImage(@DrawableRes int resId, int placeholderResId); 
    public GlideImageView loadLocalImage(String localPath, int placeholderResId);
    
#### 一行代码加载来自网络、res、SDCard中图片成圆形

    public GlideImageView loadCircleImage(String url, int placeholderResId); 
    public GlideImageView loadLocalCircleImage(int resId, int placeholderResId);
    public GlideImageView loadLocalCircleImage(String localPath, int placeholderResId);
    
#### 如果你觉得上面的方法还不能满足你，那么你可以通过下面的方法追加自己想要的属性来加载图片

    RequestOptions requestOptions(int placeholderResId);
    RequestOptions circleRequestOptions(int placeholderResId);
    
    GlideImageView load(int resId, RequestOptions options);
    GlideImageView load(Uri uri, RequestOptions options);
    GlideImageView load(String url, RequestOptions options);
    
#### 如果你还是觉得得不到满足，好吧，我提供了[GlideImageLoader](https://github.com/sfsheng0322/GlideImageView/blob/master/GlideImageView/src/main/java/com/sunfusheng/glideimageview/GlideImageLoader.java)类加载图片，比如这样加载图片：先加载缩略图再加载高清图片，并监听加载的进度

    private void loadImage(String image_url_thumbnail， String image_url) {
        RequestOptions requestOptions = glideImageView.requestOptions(R.color.black)
                .centerCrop()
                .skipMemoryCache(true) // 跳过内存缓存
                .diskCacheStrategy(DiskCacheStrategy.NONE); // 不缓存到SDCard中

        glideImageView.getImageLoader().setOnGlideImageViewListener(image_url, new OnGlideImageViewListener() {
            @Override
            public void onProgress(int percent, boolean isDone, GlideException exception) {
                progressView.setProgress(percent);
                progressView.setVisibility(isDone ? View.GONE : View.VISIBLE);
            }
        });

        glideImageView.getImageLoader().requestBuilder(image_url, requestOptions)
                .thumbnail(Glide.with(ImageActivity.this) // 加载缩略图
                        .load(image_url_thumbnail)
                        .apply(requestOptions))
                .transition(DrawableTransitionOptions.withCrossFade()) // 动画渐变加载
                .into(glideImageView);
    }
  
<br/>
    
<img src="http://ourv0v3ed.bkt.clouddn.com/GlideImageView4.gif">

<br/>

#### 该库提供两种监听加载图片进度的Listener，总有一款是你想要的

    public interface OnGlideImageViewListener {
        void onProgress(int percent, boolean isDone, GlideException exception);
    }
    
    public interface OnProgressListener {
        void onProgress(String imageUrl, long bytesRead, long totalBytes, boolean isDone, GlideException exception);
    }
        
<br/>

### [GitHub地址](https://github.com/sfsheng0322/GlideImageView)






