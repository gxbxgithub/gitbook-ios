# iOS推送

iOS10 之后统一 UserNotifications.framework

* 需兼容低版本机型
* UNUserNotificationCenter 单例管理全部推送

兼容

```
if (@available(iOS 10.0, *)) {
    UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
    center.delegate = self;
    [center requestAuthorizationWithOptions:UNAuthorizationOptionBadge | UNAuthorizationOptionSound completionHandler:^(BOOL granted, NSError *_Nullable error) {
        if (granted) {
            dispatch_async(dispatch_get_main_queue(), ^{
                   [[UIApplication sharedApplication] registerForRemoteNotifications];
            });
        }
    }];
} else {
    //对推送配置页面的设置
    UIUserNotificationSettings *userSetting = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert categories:nil];
    [[UIApplication sharedApplication] registerUserNotificationSettings:userSetting];
    //注册推送
    [[UIApplication sharedApplication] registerForRemoteNotifications];
}
```



