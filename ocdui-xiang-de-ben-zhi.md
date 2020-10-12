# OC的本质

OC 代码底层都是 C/C++ 代码

* OC -&gt; C/C++ -&gt; 汇编语言 -&gt; 机器语言
* OC 面向对象都是基于 C/C++ 的数据结构（结构体）实现的

```
技巧：将 OC 代码转为 C/C++ 代码
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc OC源文件 -o 输出的CPP文件
```



