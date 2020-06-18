# Object C 开发常见bug分析

## 配置
>文件引入，配置等bug


## 开发coding常见bug

- unrecognized selector sent to instance
错误分析：
 1. 绑定协议的监听器，没有实现协议中的方法，或者方法名写错。

- Assertion failure in -[UIApplication _runWithMainScene:transitionContext:completion:]
'NSInternalInconsistencyException', reason: 'Application windows are expected to have a root view controller at the end of application launch'

 1. UINavigationController中push指向的Controller出问题。

- warning: could not execute support code to read Objective-C class data in the process. This may reduce the quality of type information available.

 1. 重写set方法中使用self.属性名给属性赋值，造成死循环。

- Terminating app due to uncaught exception 'NSRangeException', reason: '*** 

 1.数组越界访问，循环遍历的时候出错
 
 
 
