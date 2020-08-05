### 1. Category 原理

```
struct _category_t {
    const char *name;
    struct _class_t *cls;
    const struct _method_list_t *instance_methods; // 对象方法列表
    const struct _method_list_t *class_methods; // 类方法列表
    const struct _protocol_list_t *protocols; // 协议列表
    const struct _prop_list_t *properties; // 属性列表
};
```

在程序运行时，运用运行时机制：

1. 会将所有分类的对象方法，附加到类对象方法列表中；
2. 所有分类的类方法，附加到对应元类对象方法列表中；
3. 所有分类的属性，附加到类对象的属性列表中；
4. 所有分类的协议，附加到类对象的协议列表中。

内存变化：

1. 扩容：系统会根据分类的多少扩大内存空间；
2. 挪动（内存挪动memmove）：将原来的方法列表挪动到最后面；
3. 拷贝（内存拷贝memcpy）：将分类方法列表拷贝到原来方法列表的位置。

Categroy 的加载处理过程：

1. 通过 Runtime 加载某个类的所有 Category 数据；
2. 把所有 Category 的方法、属性、协议数据，合并到一个大数数组中；
   * 后面参与编译的 Category 数据，会放在数组的前面
3. 将合并后的分类数据（方法、属性、协议），插入到类原来数据的前面。

Category 和 Class Extension 的区别是什么？

* Class Extension 在编译的时候，它的数据就已经包含在类信息中
* Category 是在运行时，才会将数据合并到类信息中

### 2. + load

1. load 方法会在 runtime 加载类、分类时调用；
2. 每个类、分类的 + load ，在程序运行过程中只调用一次；
3. 调用顺序：
   * 先调用类的 +load
     * 按照编译先后循序调用（先编译，先调用）
     * 调用子类的 + load 之前会先调用父类的 + load
   * 再调用分类的 + load
     * 按照编译的先后顺序调用（先编译，先调用）
4. load 方法是根据方法地址直接调用，并不是经过 objc\_msgSend 函数调用

### 3. + initialize

1. initialize 方法会在类第一次接收到消息时调用；
2. 调用顺序：
   * 先调用父类的 + initialize，再调用子类的 + initialize（先初始化父类，再初始化子类，每个类只会初始化1次）
3. initialize 和 + load 的很大区别是，+ initizlize 是通过 objc\_msgSend 进行调用的，所以有以下特点：

   * 如果子类没有实现 + initialize，会调动父类的 + initialize（所以父类的 + initialize 可能会被调用多次）
   * 如果分类实现了 + initialize，就覆盖类本身的 + initialize 调用

4. 伪代码：

   ```
   BOOL studentInitialized = NO;
   BOOL personInitialized = NO;

   if (!studentInitialized) {
       if (!personInitialized) {
           objc_msgSend([MJPerson class], @selector(initialize));
           personInitialized = YES;
       }
       objc_msgSend([MJStudent class], @selector(initialize));
       studentInitialized = YES;
   }
   ```


