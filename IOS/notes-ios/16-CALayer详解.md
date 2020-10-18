# CALayer

## CALayer是什么
>[官方文档](https://developer.apple.com/documentation/quartzcore/calayer)

## contents属性
**contentsScale**
- [理解contentsScale](https://joeshang.github.io/2015/01/10/2015-01-10-understand-contentsscale/)

**masksToBounds**
规定超出frame的部门不显示

**寄宿图的绘制方法**
- contents 直接赋值 (__bridge id)image.CGImage;

## 图形几何学
**三个重要的布局属性**
UIView：
- frame
- bounds
- center

Layer：
- frame
- bounds
- position

**锚点**
- anchorPoint

**拉伸过滤**
- 没看懂

**组透明**
- CAlayer.shouldRasterize属性实现图层及其子图层都会被整合成一个整体的图片


## 参考文献
- [1](https://www.jianshu.com/p/55e0fa27f817)
- [2](https://www.jianshu.com/p/079e5cf0f014)
- [3](https://juejin.im/post/6844903762180456455)
- [4](https://www.jianshu.com/p/7bf1e06c6a98)
- [5](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW1)