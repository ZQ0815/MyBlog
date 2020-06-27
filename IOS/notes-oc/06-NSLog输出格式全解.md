# NSLog输出格式大全
```
//
//  ViewController.m
//  NSLog日志输出格式大全
//
//
//
//

#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

/*
     %@           OC对象

     %d,%i        整型 (%i的老写法)

     %hd          短整型

     %ld,%lld     长整型

     %u           无符整型

     %f           浮点型和double型

     %0.2f        精度浮点数，只保留两位小数

     %x:       为32位的无符号整型数(unsigned int),打印使用数字0-9的十六进制,小写a-f;

     %X:       为32位的无符号整型数(unsigned int),打印使用数字0-9的十六进制,大写A-F;

     %o            八进制

     %zu/%zd       size_t/NSInteger

     %p            指针地址

     %e            float/double（科学计数）

     %g            float/double（科学计数）

     %s            char *  字符串

     %.*s          Pascal字符串

     %c            char 字符

     %C            unichar

     %Lf           64位double

     %lu           sizeof(i)内存中所占字节数

     NSStringFromCGAffineTransform()
     NSStringFromCGPoint()
     NSStringFromCGRect()
     NSStringFromCGSize()
     NSStringFromCGVector()
     NSStringFromClass()
     NSStringFromProtocol()
     NSStringFromRange()
     NSStringFromSelector() //sel_getName()也可以
     NSStringFromUIEdgeInsets()
     NSStringFromUIOffset()

*/



    /** 几种不同的打印函数 **
    NSLog(<#NSString * _Nonnull format, ...#>)     --> OC
    printf(<#const char *restrict, ...#>)          --> C
    CFShow(<#CFTypeRef obj#>)                      --> Core Foundation
    CFShowStr(<#CFStringRef str#>)                 --> Core Foundation
     */

    NSLog(@"当前方法名：%@",NSStringFromSelector(_cmd));
    NSLog(@"当前方法名: %s",sel_getName(_cmd));
    NSLog(@"[类 方法]：%s",__func__);
    NSLog(@"[类 方法]：%s",__FUNCTION__);
    NSLog(@"当前类名：%@",NSStringFromClass([self class]));
    NSLog(@"当前行号：%d",__LINE__);

    NSLog(@"当前文件存储路径：%s",__FILE__);
    NSString *pathStr = [NSString stringWithCString:__FILE__ encoding:NSUTF8StringEncoding]; //将CString -> NSString
    NSLog(@"当前文件名：%@",[pathStr lastPathComponent]);
    ;


    NSLog(@"当前日期：%s",__DATE__);
    NSLog(@"当前时间：%s",__TIME__);
    NSLog(@"当前App运行要求的最低ios版本：%d",__IPHONE_OS_VERSION_MIN_REQUIRED);  //Develop Target: 已选8.0
    NSLog(@"当前App支持的最高ios版本：%d",__IPHONE_OS_VERSION_MAX_ALLOWED);    //Develop Target: 最高9.0
    NSLog(@"打印__IPHONE_7_0：%d",__IPHONE_7_0);  //打印ios7.0

    NSLog(@"当前线程：%@",[NSThread currentThread]);
    NSLog(@"主线程：%@",[NSThread mainThread]);
    NSLog(@"当前栈信息：%@", [NSThread callStackSymbols]);



    //SEL对象即一个@selector对象,保存一个方法的地址
    SEL sel0 = _cmd; //代表当前方法
    SEL sel1 = @selector(sayHello);//将sayHello方法包装成SEL对象
    SEL sel2 = NSSelectorFromString(@"sayHello"); //从方法名字符串创建SEL对象
    [self sayHello];
    [self performSelector:sel1];
    [self performSelector:sel2 withObject:@"123"];

}

- (void)sayHello {

    NSLog(@"Hello, world!");

}

@end
```