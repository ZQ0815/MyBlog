# String、StringBuffer、StringBuilder
- String 字符串常量（不可变）
- StringBuffer 字符串变量（线程安全）
- StringBuilder 字符串变量（非线程安全）

## String
>String底层是一个final类型的字符数组,每次对String的操作都会生成新的String对象，造成内存浪费.
>[String详解](https://www.zhihu.com/question/29884421)

## StringBuffer
>继承AbstractStringBuilder，同时是线程安全。底层是一个动态数组。

## StringBuilder
>继承AbstractStringBuilder，不是线程安全。底层是一个动态数组。


## 区别
String 是不可变的对象。在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，非常耗费性能。

StringBuffer 则是每次都会对 StringBuffer 对象本身进行操作。

另外像这种的 String S1 = "hello" + "java"; 是直接编译时拼接的。

```
@Test
public void test() {
	String s = "hellojava";
	String s1 = "hello";
	String s2 = "java";
	// 编译时就会将字符串拼接，和s没有区别
	String s3 = "hello" + "java";
	// 只有有变量参与字符串拼接，
	// 那么就会调用StringBuilder中的toString方法
	// 创建一个新的Sting对象。
	String s4 = s1 + "java";
	String s5 = "hello" + s2;
	String s6 = s1 + s2;
	// 直接去内存中的常量池中获取该字符串对象
	String s7 = s6.intern();
	System.out.println(s == s3); // true
	System.out.println(s == s4); // false
	System.out.println(s == s5); // false
	System.out.println(s == s6); // false
	System.out.println(s4 == s5); // false
	System.out.println(s4 == s6); // false
	System.out.println(s == s7); // true
}
```
1. 在字符串不经常发生变化的业务场景优先使用String(代码更清晰简洁)。
2. 在单线程情况下，如有大量的字符串操作情况，应该使用StringBuilder来操作字符串。
3. 在多线程情况下，如有大量的字符串操作情况，应该使用StringBuffer。

## 关于String对象为什么不可变？
>[String对象为什么不可变](https://www.zhihu.com/question/31345592)
>[为什么String不可变](https://www.jb51.net/article/184283.htm)