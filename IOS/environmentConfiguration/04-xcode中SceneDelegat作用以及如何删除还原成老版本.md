# Xcode 11 新建工程中的SceneDelegate
Xcode 11 建新工程默认会创建通过 UIScene 管理多个 UIWindow 的应用，工程中除了 A
ppDelegate 外还会有一个 SceneDelegate，这是为了实现iPadOS支持多窗口的结果。AppDelegate.h不再有window属性，window属性被定义在了SceneDelegate.h中，AppDelegate中有新增的关于scene的代理方法，SceneDelegate中也有相应的代理方法。因此，当我们用Xcode11针对不同版本的iOS开发应用时，就要做一些适配。

## 如何还原项目为之前版本接着使用
1. 删除SceneDelegate的两个文件
2. 删除AppDelegate中关于Scene的两个生命周期方法
3. 在AppDelegate中手动添加UIWindow * window属性
4. 删除main.storyboard文件同时删除Info.plist文件中的Main storyboard file base name Main

## 后续探究SceneDelegate的使用方式

## 参考文档
>[reference-1](https://www.jianshu.com/p/20df9eb8c913)

