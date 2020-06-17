# Property & synthesize

## @property

**本质**
```
@property = ivar(实例变量) + getter/setter（存取方法）;
```
在正规的 Objective-C 编码风格中，存取方法有着严格的命名规范。 正因为有了这种严格的命名规范，所以 Objective-C 这门语言才能根据名称自动创建出存取方法。
完成属性定义后，编译器会自动编写访问这些属性所需的方法，此过程叫做“自动合成”(autosynthesis)。需要强调的是，这个过程由编译 器在编译期执行，所以编辑器里看不到这些“合成方法”(synthesized method)的源代码。
@property有两个对应的词，一个是 @synthesize，一个是 @dynamic。如果 @synthesize和 @dynamic都没写，那么默认的就是@syntheszie var = _var;

## @synthesize
@synthesize表示如果属性没有手动实现setter和getter方法，编译器会自动加上这两个方法。

## @dynamic

@dynamic 告诉编译器：属性的 setter 与 getter 方法由用户自己实现，不自动生成。假如一个属性被声明为 @dynamic var，而且你没有提供 @setter方法和 @getter 方法，编译的时候没问题，但是当程序运行到 instance.var = someVar，由于缺 setter 方法会导致程序崩溃；或者当运行到 someVar = var 时，由于缺 getter 方法同样会导致崩溃。编译时没问题，运行时才执行相应的方法，这就是所谓的动态绑定。

## 问题分析
### 1. 使用@property声明属性后，需要重写其set和get方法，且想要在set/get方法中使用_name来获取对象的属性。
@property 默认提供 var: _name、methods: name()、setName();
但是如果想要在重写set/get方法中使用_name需要使用@synthesize name = _name;来像.m文件中添加属性。
因为@property默认给该属性生成getter和setter方法，当getter和setter方法同时被重写时，则系统就不会自动生成getter和setter方法了，也不会自动帮你生成_num变量，所以不会识别。
