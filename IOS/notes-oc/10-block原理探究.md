# block原理探究
## 什么是Blocks
block本质上也是一个OC对象，它内部也有个isa指针。一句话概括是带有自动变量的匿名函数。

**Block语法**
^ 返回值类型 参数列表 表达式

**Blcok类型变量**
```
int (^block)(int) = ^(int) {

}
```
## Block捕获变量
1、Block捕获变量常见问题分析
- 直接给回调中的参数赋值：name = names;

**参考文献**
[Block捕获变量具体过程详解](https://cloud.tencent.com/developer/article/1517593)

## Block循环引用的问题
1. 解决codereview提的问题
2. 还在继续整理埋点这一块的wiki文档