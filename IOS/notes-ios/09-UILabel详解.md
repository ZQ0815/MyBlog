# UILabel详解

## 自定义富文本格式

- NSMutableAttributedString

```
    NSMutableAttributedString *attri = [[NSMutableAttributedString alloc] initWithString:_showInfoLabel.text];
    
    NSRange delete_Range = NSMakeRange(0, 1);
    [attri addAttribute:NSStrikethroughStyleAttributeName value:@(NSUnderlinePatternSolid | NSUnderlineStyleSingle) range:delete_Range];
    
    NSRange bigSize_Range = NSMakeRange(1, 1);
    [attri addAttribute:NSFontAttributeName value:[UIFont systemFontOfSize:20] range:bigSize_Range];
    
    NSRange redColor_Range = NSMakeRange(2, 1);
    [attri addAttribute:NSForegroundColorAttributeName value:[UIColor redColor] range:redColor_Range];
    
    NSRange redBackgroundColor_Range = NSMakeRange(3, 1);
    [attri addAttribute:NSBackgroundColorAttributeName value:[UIColor redColor] range:redBackgroundColor_Range];
```


## UILabel属性详解
[UILabel属性详解](https://juejin.im/post/6844903567149498382)