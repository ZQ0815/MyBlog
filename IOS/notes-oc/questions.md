# object c中所有对象皆为指针，和java中的对象使用的区别是什么？
# 是否所有的类定义都需要继承NSObject嘛？
# 继承父类的类在初始化的时候的细节问题
```
# 接口的定义
@interface Volume : NSObject
{
    int val;
    int min,max,step;
}
- (id)initWithMin:(int) a max:(int) b step:(int) s;
- (int) value;
- (id) up;
- (id) down;
@end

# 接口的实现
@implementation Volume
- (id)initWithMin:(int) a max:(int) b step:(int) s
{
    self = [super init]; 
    if (self != nil ){
        val = min = a;
        max = b;
        step = s;
    }
}

- (int) value
{
    return value;
}

- (id) up
{
    if((val += step) > max)
        val = max;
    return self;
}

- (id) down
{
    if((val -= step) < max)
        val = min
    return self;
}
@end
```
# self = [super init] 为什么这里需要初始化父类的信息。如果在java要实现继承的话，是否会自动继承,具体的实现的细节是什么
# 对于一些没有返回值的方法，直接返回当前对象的这种做法的好坏？
# 对于混合编程语言选择编程的风格，C和OC方法和类的设计结合的时候如何选择呢？
# 类的继承，类文件之间的引入关系？在项目的开发过程中对于头文件和源文件的管理问题
# 方法调用的原理，底层指令的研究。
# 类继承，方法重写后，super、self之间的区别。
# 子类使用父类中的变量，无法改变其值。
声明类的时候使用的是id，需要使用类指针作为静态类型。
# id作为临时变量和使用强转的区别。