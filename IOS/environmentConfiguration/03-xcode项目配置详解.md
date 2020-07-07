# xcode项目配置详解
>[xcode项目配置详解](https://www.cnblogs.com/mukekeheart/p/8315229.html)

## Target
一个Target确定了一个product to build，包括一些instruction(怎么从一个project或者workspace的一堆文件导出一个产品)。一个Target对应一个product，它管理着一个product的build system的“输入”（一堆源文件和一些处理这些源文件的instruction）。一个project可以包含多个target，每一个targe生成一个product。

一个Target和它的product可以和其他Target联系，如果一个target build需要另一个target的“输出”，可以说成第一个target依赖第二个。如果这俩个target在同一个workspace，Xcode会发现他们的依赖关系，从而build the products按照特定的顺序。这样的关系被称为“ implicit dependency.” 你也可以为俩个targets指定明确的target 依赖关系在build setting里面。例如，你可能build一个library和一个链接这个library的application（同一个workspace）。Xcode可以发现这种关系并且自动build这个library first。然而，你如果要去链接library的某个版本而不是one built in the workspace，你可以在build settings里创建一个确定的依赖关系，它将会覆盖implicit dependency。

## project
一个project是所有文件、资源、信息/配置的reposition。


## Build Setting
a build Setting是一个变量，包含着怎么build product的处理信息。例如，Xcode传给编译器的选项。



