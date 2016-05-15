---
layout: post
author: 孙福生
title: Swift 学习之 Array、Dictionary、Set
cover: "zzz"
categories: Swift
tags: Technology
---

编程的世界里，数组和集合都是开发者学习的必经之路，Swift学习亦是如此，先看下它们的特点：  
数组(Array)：有序、有越界问题  
字典(Dictionary)：无序、键值对集合  
集合(Set)：无序、唯一性、集合操作、快速查找  
下面具体看看 Swift 集合到底有多强大，能多大程度上解放劳动力。

### 数组(Array)

#### 初始化  

    // 静态初始化
    var arr = ["iOS", "Android"]

    // 动态初始化
    var arr1 = Array<String>()
    var arr2:Array<String> = []
    var arr3 = [String]()
    var arr4:[String] = []

    // 有默认值的初始化
    var arr5 = Array<String>(count: 6, repeatedValue: "")
    var arr6 = [String](count: 6, repeatedValue: "")

数组的基本操作

    // 数组长度
    arr.count

    // 数组第一个和最后一个元素
    arr.first
    arr.last

    // 数组判空
    arr.isEmpty

延伸一: 数组arr1和Java列表的初始化有点像哈
    
    // Swift Array
    var arr1 = Array<String>()
    // Java List
    List<String> list = new ArrayList<String>();

延伸二: Java数组初始化

    // 静态初始化
    String[] arr = {"iOS", "Android"};

    // 动态初始化
    String[] arr1 = new String[2];
    String[] arr2 = new String[]{"iOS", "Android"};

#### 增、删、改、查

    // 增加一个元素
    arr.append("Windows")
    arr.append("Mac")
    // 在指定位置上插入数据
    arr.insert("Linux", atIndex: 2)
    print(arr)
    // 输出
    ["iOS", "Android", "Linux", "Windows", "Mac"]
    // 这样增加也可以
    arr += arr
    arr += ["OS X"]

    // 删除指定位置的元素
    arr.removeAtIndex(3)
    // 删除第一个元素
    arr.removeFirst()
    // 删除最后一个元素
    arr.removeLast()
    // 删除所有元素
    arr.removeAll()

    // 修改指定元素或制定范围的数据
    arr[4] = "MAC"
    arr[2..<arr.count] = ["Mac"]
    print(arr)
    // 输出
    ["iOS", "Android", "Mac"]

    // 方式一：遍历数组
    for index in 0..<arr.count {
        print(arr[index])
    }
    // 方式二：遍历数组
    for index in arr {
        print(index)
    }


### 字典(Dictionary)  
字典的主要特点就是无序的键－值对集合，表现形式是 key:value

#### 初始化

    var dict = ["name":"sunfusheng", "age":20, "blog":"sunfusheng.com"]

    // 初始化空的字典
    var dict1:Dictionary<String, String> = [:]
    var dict2:[String:String] = [:]
    var dict3 = [String:String]()
    var dict4 = Dictionary<String, String>()

#### 增、删、改、查

    // 有则更新键值对的值，无则增加一个键值对
    dict["github"] = "sfsheng0322"
    // updateValue: 有则更新键值对的值，无则增加一个键值对，返回 oldValue
    dict.updateValue("孙福生微博", forKey: "sina")
    dict.updateValue(28, forKey: "age")
    print(dict)
    // 输出
    ["age": 28, "blog": sunfusheng.com, "github": sfsheng0322, "sina": 孙福生微博, "sex": 1, "name": sunfusheng]

    // 删除指定键的键值对，没有这个键值对相当于无操作
    dict.removeValueForKey("sex")
    // 删除所有的键值对
    dict.removeAll()

    // 字典键的集合
    print(Array(dict.keys))
    // 输出
    ["age", "blog", "github", "sina", "name"]
    // 字典值的集合
    print(Array(dict.values))
    // 输出
    [28, sunfusheng.com, sfsheng0322, 孙福生微博, sunfusheng]

    // 遍历字典的键
    for key in dict.keys {
        print(key)
    }
    // 遍历字典的值
    for value in dict.values {
        print(value)
    }
    // 使用元祖遍历字典的键值，for in 真强大！Swift 真强大！
    for (key, value) in dict {
        print("\(key):\(value)")
    }

### 集合(Set)  
集合(Set)特点：无序、唯一性、集合操作、快速查找。  
集合的数据显示和数组是一样的，所以必须显示的声明Set集合！不然！就是数组！

#### 初始化

    // 初始化A、B、C三个集合
    var A:Set<String> = ["A", "B", "C", "D"]
    var B:Set<String> = ["C", "D", "E", "F"]
    var C:Set<String> = ["B", "B", "C"]

    // 上面Array、Dictionary动态初始化有四种，这里面只有两种
    var set1 = Set<String>()
    var set2:Set<String> = []

集合的基本操作

    // 集合的数量
    A.count
    // 集合第一个元素，因为集合的无序性，它的第一个元素没有意义
    A.first
    // 集合是否为空
    A.isEmpty
    // 向集合中插入一个元素
    A.insert("C")
    // 删除集合中的元素
    A.remove("C")
    // 判断集合中是否包含某个元素
    A.contains("A")
    // 遍历集合
    for index in A {
        index
    }

#### 集合操作

A.union(B): A、B的值不改变  
A.unionInPlace(B): A是并集后的值，改变；B的值不变  
并集、交集、异或均是如此

    // 为了方便大家看清楚输出结果，我将结果有序排列，但要知道集合的无序性哦
    A = ["A", "B", "C", "D"]
    B = ["C", "D", "E", "F"]
    C = ["B", "B", "C"]

    // 并集 (A∪B)
    A.union(B)
    A.unionInPlace(B)
    // 输出
    Set A: ["A", "B", "C", "D", "E", "F"]
    Set B: ["C", "D", "E", "F"]

    // 交集 (A∩B)
    A.intersect(B)
    A.intersectInPlace(B)
    // 输出
    Set A: ["C", "D"]

    // 异或 (A^B)
    A.exclusiveOr(B)
    A.exclusiveOrInPlace(B)
    // 输出
    Set A: ["A", "B", "E", "F"]

    // C 是否是 A 的子集
    C.isSubsetOf(A)
    // A 是否是 C 的超集
    A.isSupersetOf(C)

今天的内容看起来很多，其实就那些操作，增删改查；由于Xcode强大的提示功能，我们不需要记住那些函数的名字，用的时候去找就OK，但它们的特性还是要记住的，假以时日多加练习，必将游刃有余，信手捏来；大家加油！













