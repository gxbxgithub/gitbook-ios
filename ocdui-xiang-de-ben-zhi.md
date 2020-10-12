# OC的本质

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
NSLog(@"%zd", class_getInstanceSize([NSObject class]));

// 获得 obj 指针所指向的内存的大小 >> 16
NSLog(@"%zd", malloc_size((__bridge const void *)(obj)));
```



