---
layout: post
author: 孙福生
title: Swift 学习之可选型(Optional)
cover: "zzz"
categories: Swift
tags: Technology
---

最近学习Swift，也在Playground上敲了些代码，虽然说Swift类似于Java，可从Java转过来后我还是习惯地判空，像下面这个简单的判断:  
    String name = "sunfusheng";  
    if (name != null) {  
        // balabala...  
    }  
但是在Swift中却不行，因为什么呢？原因在于Swift是强类型语言，注重代码的健壮与安全。那我还想判空，怎么解？学到Swift的可选型问题迎刃而解。

### 可选型(Optional)

    public enum Optional<T> {
        case None
        case Some(T)
    ｝

可以看出可选型是个泛型，而Optional又是个枚举 enum，Optional可以是 None，也可以是Some(T)。

### 声明可选型

显式声明  
"?" 是 Optional\<T> 的简写形式。

    var name:Optional<String> = "sunfusheng"
    var blog:String? = "sunfusheng.com"

    // 现在就可以判断啦
    if name != nil {
        print(name)
    }
    print(blog)

    // 输出
    Optional("sunfusheng")
    Optional("sunfusheng.com")

隐式声明  
"!" 是 ImplicitlyUnwrappedOptional\<T> 的简写形式。

    var name1:ImplicitlyUnwrappedOptional<String> = "sunfusheng"
    var blog1:String! = "sunfusheng.com"

    print(name1)
    print(blog1)

    // 输出
    sunfusheng
    sunfusheng.com

### 可选型解包

Swift 中 String 和 Optional\<String> 不是一个类型的，因为 Swift 是强类型语言，String 类型 和 Optional\<String> 类型数据拼接肯定报错的，所以我们需要解包后使用。反过来说，Java 是弱类型语言，随便加，随便拼接。

强制解包

    // 显式声明的可选型变量后加 "!" 强制解包
    var name:String? = "sunfusheng"
    "My name is " + name!

    // 不建议使用强制解包，下面就会报错
    name = nil
    "My name is " + name!

    // 隐式声明的可选型不需要解包，同样可能因为 nil 报错
    var blog:String! = "sunfusheng.com"
    // blog = nil
    "My blog is " + blog

if let 解包可选型

    // 可以使用相同的变量名解包
    if let errorCode = errorCode {
        print(errorCode)
    }

    // if let 可以同时解包多个变量，同样可以使用控制转移的条件
    if let errorCode = errorCode, errorMessage = errorMessage where errorCode == "404" {
        print("Page not found")
    } else {
        print("No error")
    }

### 可选链 (Optional Chaining)  
使用可选链 (Optional Chaining) 的方式来调用可能为空（nil）的方法，比如下面的代码

    // 把我的名字全变成大写字母
    var name:String? = "sunfusheng"
    if let name = name {
        print(name.uppercaseString)
    }
    // 输出
    SUNFUSHENG

    // 更优雅地使用可选链实现
    print(name?.uppercaseString)
    // 输出
    SUNFUSHENG

    // 如果 name = nil 就不会继续执行 uppercaseString
    name = nil
    print(name?.uppercaseString)
    // 输出
    nil

当然也可以使用强制解包方式使用可选链，当然不建议使用，除非你确保它不为 nil

    print(name!.uppercaseString)
    // 输出
    SUNFUSHENG

    // 编译不过，报错
    name = nil
    print(name!.uppercaseString)

可选型的强大之处可见一斑，以后的开发必然会经常用到的。















