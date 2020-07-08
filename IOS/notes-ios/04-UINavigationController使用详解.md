# UINavigationController使用详解

## 属性分析
```
@interface UINavigationController : UIViewController
@property(nonatomic,readonly) UINavigationBar *navigationBar;

@interface UIViewController (UINavigationControllerItem)
@property(nonatomic,readonly,strong) UINavigationItem *navigationItem;


@interface UINavigationBar : UIView
@property(nullable, nonatomic,readonly,strong) UINavigationItem topItem;
@property(nullable, nonatomic,readonly,strong) UINavigationItem backItem;
@property(nullable,nonatomic,copy) NSArray<UINavigationItem > items;


@interface UINavigationItem : NSObject
@property(nullable,nonatomic,strong) UIBarButtonItem backBarButtonItem
@property(nullable,nonatomic,copy) NSArray<UIBarButtonItem > leftBarButtonItems;
@property(nullable,nonatomic,copy) NSArray<UIBarButtonItem > rightBarButtonItems;
@property(nullable, nonatomic,strong) UIBarButtonItem leftBarButtonItem;
@property(nullable, nonatomic,strong) UIBarButtonItem *rightBarButtonItem;


@interface UIBarButtonItem : UIBarItem
@property(nullable, nonatomic)         SEL                  action;


@interface UIBarItem : NSObject
@property(nullable, nonatomic,copy)             NSString    title;
@property(nullable, nonatomic,strong)           UIImage     image;</code></pre> 
```

## 基本属性介绍
### UIBarItem
 一个可以放置在Bar之上的所有小控件类的抽象类,可以设置标题，图片等
 
   
### UIBarButtonItem
 继承UIBarItem，增加了动作以及目标等button的属性。相当于放在UIToolBar或者UINavigationBar上的特殊的button。
 
   
### UINavigationItem
 包含了title,prompt,titleView,leftBarButtonItem,rightBarButtonItem,backBarButonItem等当前页面上所有的信息
 
   
### UINavigationBar
NavigaitonBar就是导航栏 主要对UINavigationItem进行栈管理 展示导航栏的外观背景

1. 源码分析
```
//UIBarStyleDefault  灰色背景 白色文字 UIBarStyleBlack    纯黑色背景 白色文字
@property(nonatomic,assign) UIBarStyle barStyle;
@property(nullable,nonatomic,weak) id<UINavigationBarDelegate> delegate;
//Translucent设置成透明度,设置成YES会有一种模糊效果
@property(nonatomic,assign,getter=isTranslucent) BOOL translucent NS_AVAILABLE_IOS(3_0) UI_APPEARANCE_SELECTOR; 
//UINavigationBar上面不只是简单的显示标题，它也将标题进行了堆栈的管理，每一个标题抽象为的对象在iOS系统中是UINavigationItem对象，我们可以通过push与pop操作管理item组。
//向栈中添加一个item，上一个item会被推向导航栏的左侧，变为pop按钮，会有一个动画效果

(void)pushNavigationItem:(UINavigationItem *)item animated:(BOOL)animated;
//pop一个item

(nullable UINavigationItem )popNavigationItemAnimated:(BOOL)animated; 
//当前push到最上层的item
@property(nullable, nonatomic,readonly,strong) UINavigationItem topItem;
//仅次于最上层的item，一般式被推向导航栏左侧的item
@property(nullable, nonatomic,readonly,strong) UINavigationItem backItem;
//获取堆栈中所有item的数组
@property(nullable,nonatomic,copy) NSArray<UINavigationItem > *items;
//设置一组item

(void)setItems:(nullable NSArray<UINavigationItem > )items animated:(BOOL)animated; 

//系统类型按钮文字颜色
@property(null_resettable, nonatomic,strong) UIColor *tintColor;

//通过barTintColor来设置背景色
@property(nullable, nonatomic,strong) UIColor *barTintColor NS_AVAILABLE_IOS(7_0) UI_APPEARANCE_SELECTOR;

//设置工具栏背景和阴影图案
(void)setBackgroundImage:(nullable UIImage *)backgroundImage forBarPosition:(UIBarPosition)barPosition barMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(7_0) UI_APPEARANCE_SELECTOR;

(nullable UIImage *)backgroundImageForBarPosition:(UIBarPosition)barPosition barMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(7_0) UI_APPEARANCE_SELECTOR;

//通过背景图片来设置导航栏的外观

(void)setBackgroundImage:(nullable UIImage *)backgroundImage forBarMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(5_0) UI_APPEARANCE_SELECTOR;

(nullable UIImage *)backgroundImageForBarMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(5_0) UI_APPEARANCE_SELECTOR;

//背景阴影图片 - 即分割线
@property(nullable, nonatomic,strong) UIImage *shadowImage NS_AVAILABLE_IOS(6_0) UI_APPEARANCE_SELECTOR;

//标题的富文本
@property(nullable,nonatomic,copy) NSDictionary<NSString ,id> titleTextAttributes NS_AVAILABLE_IOS(5_0) UI_APPEARANCE_SELECTOR;

//标题垂直偏移
(void)setTitleVerticalPositionAdjustment:(CGFloat)adjustment forBarMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(5_0) UI_APPEARANCE_SELECTOR;

(CGFloat)titleVerticalPositionAdjustmentForBarMetrics:(UIBarMetrics)barMetrics NS_AVAILABLE_IOS(5_0) UI_APPEARANCE_SELECTOR;

//设置返回按钮的图片
@property(nullable,nonatomic,strong) UIImage backIndicatorImage NS_AVAILABLE_IOS(7_0) UI_APPEARANCE_SELECTOR;
@property(nullable,nonatomic,strong) UIImage backIndicatorTransitionMaskImage NS_AVAILABLE_IOS(7_0) UI_APPEARANCE_SELECTOR;
@end</code></pre> 
```

2. 视图组成
- backIndicatorImage: 返回按钮的图片
- backBarButtonItem: 返回文字内容
- leftBarButtonItem: 左侧按钮选项
- rightBarButtonItem: 右侧推进按钮选项
   
### UINavigationController
 包含了viewcontrollers、navigationbar、toolbar

 ## 使用注意事项
 1. 当UINavigationController作为window的rootViewController，且UITabBarController作为UINavigationController的根控制器时想要在tabBar的viewController中直接修改navigationItem.title的内容时，需要在viewWillAppear生命周期方法中设置，在viewDidLoad中设置无效。