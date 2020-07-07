- singleApp加载window的配置。
```
//
//  AppDelegate.m
//  zhangqiangDemo
//
//  Created by 张强 on 2020/7/7.
//  Copyright © 2020 张强. All rights reserved.
//

#import "AppDelegate.h"
#import "MineViewController.h"
#import "NewsViewController.h"
#import "FindViewController.h"

@interface AppDelegate ()

@end

@implementation AppDelegate


- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // 设置c初始window，作用代替Main.storyboard
    self.window = [[UIWindow alloc] initWithFrame: [[UIScreen mainScreen] bounds]];
    
    // 初始化所有页面
    FindViewController *findViewController = [[FindViewController alloc] init];
    NewsViewController *newsViewController = [[NewsViewController alloc] init];
    MineViewController *mineViewController = [[MineViewController alloc] init];
    
    // 定义UITabBar,并添加上述所有页面
    UITabBarController *tabController = [[UITabBarController alloc] init];
    [tabController setViewControllers:@[findViewController, newsViewController, mineViewController]];
    
    // 指定UINavigationController为rootViewController，并且指定viewController为UITabBarController的第一个视图控制器
    UINavigationController * navController = [[UINavigationController alloc] initWithRootViewController:tabController];
    self.window.rootViewController = navController;
    [self.window makeKeyAndVisible];
    return YES;
}


#pragma mark - UISceneSession lifecycle


- (UISceneConfiguration *)application:(UIApplication *)application configurationForConnectingSceneSession:(UISceneSession *)connectingSceneSession options:(UISceneConnectionOptions *)options {
    // Called when a new scene session is being created.
    // Use this method to select a configuration to create the new scene with.
    return [[UISceneConfiguration alloc] initWithName:@"Default Configuration" sessionRole:connectingSceneSession.role];
}


- (void)application:(UIApplication *)application didDiscardSceneSessions:(NSSet<UISceneSession *> *)sceneSessions {
    // Called when the user discards a scene session.
    // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
    // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
}


@end

```

```
@implementation FindViewController
- (instancetype)init {
    self = [super init];
    if (self) {
        self.tabBarItem.title = @"发现";
        self.view.backgroundColor = [UIColor whiteColor];
    }
    return self;
}
```