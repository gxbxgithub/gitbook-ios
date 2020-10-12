# OC的本质

OC 代码底层都是 C/C++ 代码

* OC -&gt; C/C++ -&gt; 汇编语言 -&gt; 机器语言
* OC 面向对象都是基于 C/C++ 的数据结构（结构体）实现的

```
技巧：将 OC 代码转为 C/C++ 代码
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc OC源文件 -o 输出的CPP文件

查看苹果源码地址：
https://opensource.apple.com/tarballs/objc4/
```

探讨 NSObject 对象占用的内存大小

```Objective-C
NSObject *obj = [[NSObject alloc] init];
        
// NSObject 实例对象的成员变量所占用的大小 >> 8
NSLog(@"%zd", class_getInstanceSize([NSObject class]));
        
// 获得 obj 指针所指向的内存的大小 >> 16
NSLog(@"%zd", malloc_size((__bridge const void *)(obj)));
```



