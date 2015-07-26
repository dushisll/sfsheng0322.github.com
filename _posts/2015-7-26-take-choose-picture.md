---
layout: post
title: 拍照和从相册选择图片
---

好久没有写博客了，感到深深的自责。这是一个以分享为乐的时代，显然这段时间我对不起这个时代了，呵呵。
现在Android市场上app的数量越来越多了，这就要求我们在开发软件和版本迭代也要快（如外包公司和创业公司就需要快速拿出产品和简单的demo原型）。
现在大多数的软件在使用时需要用户注册账号设置个人信息，而设置用户的头像是必不可少的，我们的项目也用到了这个功能。

效果图如下：

![](/img/android_take_choose_picture.png)
![](/img/android_take_choose_picture1.png)

完成这样的效果很简单，GitHub上已经实现对应的[库](https://github.com/jgilfelt/SystemBarTint)。

#### 首先，Studio下添加依赖或者引入相应的jar文件。

	compile 'com.readystatesoftware.systembartint:systembartint:1.0.3'

#### 其次，在Activity中加入如下代码：

	public void initSystemBarTint(boolean on, int res) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            setTranslucentStatus(on);
            SystemBarTintManager tintManager = new SystemBarTintManager(this);
            tintManager.setStatusBarTintEnabled(on);
            tintManager.setStatusBarTintResource(res);
        }
    }

    private void setTranslucentStatus(boolean on) {
        Window win = getWindow(); WindowManager.LayoutParams winParams = win.getAttributes();
        int bits = WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS;
        if (on) {
            winParams.flags |= bits;
        } else {
            winParams.flags &= ~bits;
        }
        win.setAttributes(winParams);
    }

#### 最后，在对应Activity的根布局中加入下面的属性即完成。

	android:fitsSystemWindows=”true” 

