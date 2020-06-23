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

## 参数列表

### 内存管理特性
- retain  
- assign  
- copy  
- strong    
- weak     
- unsafe_unretained     
- autoreleasing

1. assign: setter方法直接赋值，不进行任何retain操作，不改变引用计数。该方法只会针对“纯量类型”(CGFloat或NSInteger等)和C数据类型（int, float, double, char, 等等）的简单赋值操作，id类型也要用assign，所以一般iOS中的代理delegate属性都会用assign来标示。

2. retain：生成符合内存管理的set方法（release旧值，retain新值），适用于OC对象的成员变量。

3. copy：生成符合内存管理的set方法（release旧值，copy新值），适用于NSString、NSArray等不可变对象。和strong类似，不过该属性会被复制一个新的副本。很多时候使用copy是为了防止Mutable（可变类型）在我们不知道的情况下修改了属性值，而用copy可以生成一个不可变的副本防止被修改。如果我们自己实现setter方法的话，需要手动copy。

4. strong：强引用，其存亡直接决定了所指向对象的存亡。使用该特性实例变量在赋值时，会释放旧值同时设置新值，对对象产生一个强引用，即引用计数+1。如果不存在指向一个对象的引用，并且此对象不再显示在列表中，则此对象会被从内存中释放。

5. weak：表示的是一个弱引用，这个引用不会增加对象的引用计数，并且在所指向的对象被释放之后，weak指针会被置为nil。weak引用通常是用于处理循环引用的问题，如代理及block的使用中，相对会较多的使用到weak。即使一个对象被持有无数个弱引用，只要没有强引用指向它，那么还是会被清除。相比于assign，声明为weak的指针，指针指向的地址一旦被释放，这些指针都将被赋值为 nil。这样的好处能有效的防止野指针。一般iOS的ARC中的代理delegate属性都会用weak


### 读写特性
- readwrite
- readonly

### 多线程特性
- nonatomic
- atomic


### 方法名特性
- setter
- getter




