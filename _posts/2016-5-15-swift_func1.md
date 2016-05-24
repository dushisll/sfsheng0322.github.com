---
layout: post
author: 孙福生
title: Swift 学习之函数(Func)基础
cover: "zzz"
categories: Swift
tags: Technology
---

从编写面向过程语言的C语言开始就一直离不开函数，当然任何语言都有函数这个概念，函数是用来完成特定任务的独立的代码块。Swift是面相对象的语言，充满现代的编程气息，更显高级，下面看看苹果在Swift中对函数都有哪些高级应用。

### 函数示例

    func sayHelloTo(name:String?) -> String {
        return "Hello, " + (name ?? "sunfusheng")
    }

    print(sayHelloTo("Swift"))
    print(sayHelloTo(nil))

    // 输出
    Hello, Swift
    Hello, sunfusheng

可以提炼如下Swift函数的语法：  

    func 函数名(参数列表) -> 返回值类型 {
        // do something ... 
    }

函数名没什么好讲的，不过苹果在OC中比较推崇具有语意的函数名，所以每个系统的函数名字都比较长，虽然在Swift中苹果有所收敛，但很多依然过长，不过反过来讲这样我们在读代码的时候会比较自然，有种一气呵成的感觉，比如上面的函数 sayHelloTo 加入一个介词 To，代码读起来就会很自然，关于函数名尽量学习官方的命名就好。接下来我分别从参数列表和返回值记录Swift语言的函数操作。

#### PS：  

在 sayHelloTo 这个函数中还有 "??" 两个问号操作符，这个鬼在官方文档中也有说明，我称之为可选型的聚合操作符，看一下官方文档 

Nil Coalescing Operator  
let c = a != nil ? a! : b  

这不就是三目运算符，通过聚合操作符可以简写如下样式：  
let c = a ?? b  

解释一下上述这行代码，意思就是 c 的值是 a 或 b 中一个的值，但有前提条件，就是当 a 解包后值不为 nil 时，那么就将 a 解包后的值赋值给 c，如果 a 解包后值为 nil，那么就将 b 的值赋值给c，还有一个条件就是，b 的类型必须和 a 解包后的类型一致。

### 函数的参数

看几个简单的使用示例

    // 1.无参数函数
    func sayHello() -> String {
        return "Hello, sunfusheng"
    }

    // 2.多参数函数
    func sayHelloTo(name:String, separator:String, terminator:String) -> String {
        return "Hello" + separator + name + terminator
    }
    print(sayHelloTo("sunfusheng", separator: ",", terminator: "!"))
    // 输出
    Hello,sunfusheng!

    // 3.可变参数函数
    func sayHelloTo(names:String?..., separator:String?) -> String {
        var persons:String = ""
        for name in names {
            persons += separator ?? ","
            persons += name ?? "SunFusheng"
        }
        return "Hello" + persons
    }
    print(sayHelloTo("ZhangSan", "LiSi", nil, separator: ","))
    // 输出
    Hello,ZhangSan,LiSi,SunFusheng

    // 4.默认参数函数，使用 "=" 赋上一个默认值
    func sayHelloTo(name:String = "sunfusheng") -> String {
        return "Hello, " + name
    }
    print(sayHelloTo())
    print(sayHelloTo("sun"))
    // 输出
    Hello, sunfusheng
    Hello, sun

如果说上面这四种情况还感觉不出 Swift 函数的牛逼，那么继续往下看！

    // 5.内部参数:name
    func sayHello(name:String) -> String {
        return "Hello, " + name
    }
    // 6.外部参数:to 和内部参数:name
    func sayHello(to name:String) ->  String {
        return "Hello, " + name
    }
    sayHello("Swift")
    sayHello(to: "Swift")

在同一个文件中函数(5)和函数(6)是可以编译通过的，从而知道函数(5)和函数(6)是两个函数；函数(5)的函数名称：sayHello(_:)，而函数(6)的名称：sayHello(to:)。当然在 Swift 中函数中第一个参数的外部参数是省略的，所以我这个函数(6)这样写是违背苹果 Swift 语言的默认风格的，当然是不提倡的。不过你再想想，函数(6)加上外部参数"to"这个介词后，对于这个函数的语意是有意义的，虽有点牵强，但辩证的看问题总是好的嘛。

这段说明中引出一个屌屌的符号 "_"，下面用一下这个符号，比如下面的例子：

    // 功能就是两个整数相加
    func add(a:Int, b:Int) -> Int {
        return a + b
    }
    add(1, b:1)

    // 这！这么用！挺反人类的！加上外部参数呢？
    func add(a:Int, and b:Int) -> Int {
        return a + b
    }
    add(1, and: 1)

    // 7.还是反人类的用法！好！使用 "_" 去掉内部参数吧！
    func add(a:Int, _ b:Int) -> Int {
        return a + b
    }
    add(1, 1)

 最后，我们看看如何修改函数传入的参数，在Swift中如果想修改传入的参数值，使用关键字 inout（输入输出参数），需要注意的是：输入输出参数不能有默认值，而且可变参数不能用 inout 标记。

    // 编译错误，因为函数的参数默认是常量，不能修改
    func changeNum(num:Int) {
        num = num * 10
    }

    // 编译通过，但不能修改传入的参数值
    func changeNum(var num:Int) {
        num = num * 10
    }

    // 8.使用 inout 标记修改传入的参数值
    func changeNum(var num:Int) {
        num = num * 10
    }
    var num:Int = 1
    print("Before num:", num)
    // 调用函数，使用取地址符传入参数的地址
    changeNum(&num)
    print("After num:", num)
    // 输出
    Before num: 1
    After num: 10

上面使用了八个小例子记录并说明函数参数的使用，再看看函数返回值的使用。

### 函数的返回值

    // 1.没有返回值的函数
    func sayHello(name:String) {
        print(name)
    }

    // 2.返回值是可选型类型
    func say(content:String?) -> String? {
        guard content != nil else {
            return nil
        }
        return content
    }
    print(say(nil))
    print(say("sunfusheng.com"))
    // 输出
    nil
    Optional("sunfusheng.com")

    // 3.返回值是元祖类型
    func swap(a:Int, _ b:Int) -> (Int, Int) {
        return (b, a)
    }
    print(swap(1, 2))

    // 4.返回值是元祖类型的可选型，为方便解包后有语意加入元祖分量名称
    func findMaxMinNum(arr:[Int]) -> (max:Int, min:Int)? {
        guard !arr.isEmpty else {
            return nil
        }
        var maxValue = arr[0]
        var minValue = arr[0]
        for num in arr {
            maxValue = max(maxValue , num)
            minValue = min(minValue , num)
        }
        return (maxValue, minValue)
    }
    var mm = findMaxMinNum([10, 20, 30, 40])
    if let mm = mm {
        print("Max:", mm.max)
        print("Min:", mm.min)
    }
    // 输出
    Max: 40
    Min: 10

Swift 函数的基本用法先记录到此，越学越觉得 Swift 屌屌的！下一篇聊聊 Swift 和 Java 的函数式编程。






