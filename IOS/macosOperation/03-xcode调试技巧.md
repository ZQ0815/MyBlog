# xcode调试技巧

## 断点类型
- [断点类型介绍](https://www.jianshu.com/p/86b669181ffc)
- [断点调试](http://www.cocoachina.com/articles/12842)

**watch断点**
在Xcode的watch窗口-> 右键需要watch的变量 -> watch “XXX”
当所watch的值发生变化时调试器会自动暂停。并打印信息

**断点调试查看frame和bounds**
1. expr @import UIKit
2. po self.bounds