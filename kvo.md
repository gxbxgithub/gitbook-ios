KVO 全称 “Key-Value Observing”，俗称“键值监听”，用于监听某个对象属性值的改变。

KVO 本质：通过 Runtime 动态创建一个类 NSKVONotifying\_GPerson，是 GPerson 的子类

```cpp
GPerson *person1 = [[GPerson alloc] init];
GPerson *person2 = [[GPerson alloc] init];

NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
[self.person1 addObserver:self forKeyPath:@"name" options:options context:nil];

// self.person1.isa = NSKVONotifying_GPerson
// self.person2.isa = GPerson


/*
 * NSKVONotifying_GPerson 的伪代码实现
 */
@implementation NSKVONotifying_GPerson

- (void)setName:(NSString *)name {
  _NSSetObjectValueAndNotify();
}

void _NSSetStringValueAndNotify() {
  [self willChangeValueForKey:@"name"];
  [super setName:name];
  [self didChangeValueForKey:@"name"];
}

- (void)didChangeValueForKey:(NSString *)key {
  // 通知监听器，**属性值发生了改变
  [observer observeValueForKeyPath:key ofObject:self change:nil context:nil];
}

@end
```



