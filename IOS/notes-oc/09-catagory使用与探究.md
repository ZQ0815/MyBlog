# Object c中关于catagory的应用和原理探究

## catagory的基本使用
- 需求背景：拓展现有类的实例变量和方法。

- 简单的demo
1. 原有类(MyClass.m)：
```
@interface MyClass : NSObject 
-(void) myPrint; 
@end 
#import "MyClass.h" 
 
@implementation MyClass 
-(void) myPrint{ 
    NSLog(@"myPrint 调用了"); 
} 
@end 
```

2. 给现有类添加一个helloworld的方法(MyClass+HelloWorld.m)：
```
#import "MyClass.h" 
 
@interface MyClass (HelloWorld) 
-(void)HelloWorld; 
@end 
#import "MyClass+HelloWorld.h" 
 
@implementation MyClass (HelloWorld) 
-(void)HelloWorld{ 
    NSLog(@"你好 伦敦奥运!"); 
} 
@end 
```

3. 在main方法中调用
```
 MyClass *myclass = [[[MyClass alloc]init]autorelease]; 
[myclass HelloWorld]; 
[myclass myPrint]; 
```

## catagory的使用场景
1. 为已存在的类添加方法
2. 声明私有方法
3. 模拟多继承
4. 把framework的私有方法公开
5. 方法的前向声明
```
-(void)methodA{
    use methodB
}
-(void)methodB{
    use methodA
}
```
通过把所有方法都放到局部catagory中：
```
@interface 类名 (Local)
- (void)methodA;
- (void)methodB;
@end

@implementation 类名 (Local)
- (void)methodA{

}
- (void)methodB{

}
@end
```

## catagory使用过程中存在的疑问
1. 编译期间能否检测到catagory中声明的方法？
2. 是否引入了对应的catagory+文件后，当前文件是否能使用其方法？
3. 方法的前向声明原理


## catagory的原理探究
我们知道，所有的OC类和对象，在runtime层都是用struct表示的，category也不例外，在runtime层，category用结构体category_t（在objc-runtime-new.h中可以找到此定义），它包含了：
1. 类的名字（name）
2. 类（cls）
3. category中所有给类添加的实例方法的列表（instanceMethods）
4. category中所有添加的类方法的列表（classMethods）
5. category实现的所有协议的列表（protocols）
6. category中添加的所有属性（instanceProperties）
```
typedef struct category_t {
    const char *name;
    classref_t cls;
    struct method_list_t *instanceMethods;
    struct method_list_t *classMethods;
    struct protocol_list_t *protocols;
    struct property_list_t *instanceProperties;
} category_t;
```

## 特殊的catagory---extension

### 什么是extension
extension被开发者称之为扩展、延展、匿名分类。extension看起来很像一个匿名的category，但是extension和category几乎完全是两个东西。和category不同的是extension不但可以声明方法，还可以声明属性、成员变量。extension一般用于声明私有方法，私有属性，私有成员变量。

### extension的存在形式
category是拥有.h文件和.m文件的东西。但是extension不然。extension只存在于一个.h文件中，或者extension只能寄生于一个类的.m文件中。比如，viewController.m文件中通常寄生这么个东西，其实这就是一个extension：
```
@interface ViewController ()
 声明私有属性和方法
@end
```

### category和extension的区别
就category和extension的区别来看，我们可以推导出一个明显的事实，extension可以添加实例变量，而category是无法添加实例变量的（因为在运行期，对象的内存布局已经确定，如果添加实例变量就会破坏类的内部布局，这对编译型语言来说是灾难性的）。

- extension在编译期决议，它就是类的一部分，但是category则完全不一样，它是在运行期决议的。extension在编译期和头文件里的@interface以及实现文件里的@implement一起形成一个完整的类，它、extension伴随类的产生而产生，亦随之一起消亡。

- extension一般用来隐藏类的私有信息，你必须有一个类的源码才能为一个类添加extension，所以你无法为系统的类比如NSString添加extension，除非创建子类再添加extension。而category不需要有类的源码，我们可以给系统提供的类添加category。

- extension可以添加实例变量，而category不可以。

- extension和category都可以添加属性，但是category的属性不能生成成员变量和getter、setter方法的实现。