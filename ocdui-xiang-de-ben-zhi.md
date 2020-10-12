# OC对象的本质

OC 代码底层都是 C/C++ 代码

* OC -&gt; C/C++ -&gt; 汇编语言 -&gt; 机器语言
* OC 面向对象都是基于 C/C++ 的数据结构（结构体）实现的

```
技巧：将 OC 代码转为 C/C++ 代码
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc OC源文件 -o 输出的CPP文件

查看苹果源码地址：
https://opensource.apple.com/tarballs/objc4/

常用 LLDB 指令
1. print、p：打印
2. po：打印对象
3. 读取内存
    （1）通过指令
    memory read/[数量][格式][字节数] 内存地址  <===>  x/[数量][格式][字节数] 内存地址
    格式：
        x是16进制，
        f是浮点，
        d是10进制
    字节大小：
        b：byte 1字节，
        h：half word 2字节，
        w：work 4字节，
        g：giant word 8字节
    举例：
        x/4xw 0x10302da50
        打印：0x10302da50: 0x8de00119 0x001dffff 0x00000000 0x00000000

    （2）通过xcode：Debug -> Debug Workflow -> View Memory
```

探讨 NSObject 对象占用的内存大小

```cpp
NSObject *obj = [[NSObject alloc] init];

// NSObject 实例对象的成员变量所占用的大小 >> 8
// #import <objc/runtime.h>
NSLog(@"%zd", class_getInstanceSize([NSObject class]));

// 获得 obj 指针所指向的内存的大小 >> 16
// #import <malloc/malloc.h>
NSLog(@"%zd", malloc_size((__bridge const void *)(obj)));
```

# OC对象的分类

OC 对象主要分为3中：

* instance 对象（实例对象）
* class 对象（类对象）
* meta-class 对象（元类对象）

### instance 对象

在内存中存储的信息包括：

* isa 指针
* 其他成员变量

### class 对象

在内存中存储的信息包括：

* isa 指针
* superclass 指针
* 类的属性信息（@property）、类的对象方法信息（instance method）
* 类的协议信息（protocol）、类的成员变量信息（ivar）

### meta-class 对象

在内存中存储的信息包括：

* isa 指针
* superclass 指针
* 类的类方法信息（class method）

类对象的获取方式：

```
NSObject *object = [[NSObject alloc] init];

// 类对象的获取方式
Class objClass1 = [object class];
Class objClass2 = [NSObject class];
Class objClass3 = object_getClass(object); // #import <objc/runtime.h>

// 元类对象的获取方式
// 将类对象当做参数传入，获得元类对象
Class metaClass = object_getClass(objectClass1); // #import <objc/runtime.h>

// 判断是否是元类对象
BOOL isMetaClass = class_isMetaClass(metaClass); // #import <objc/runtime.h>
```



