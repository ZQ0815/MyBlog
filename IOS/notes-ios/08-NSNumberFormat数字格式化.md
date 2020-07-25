# 关于数字格式化类的使用

## 业务需求
在不同场景下数字的显示格式需要调整：(1000000)
- 货币：1,000,000元
- 科学计数：1E6

## NSNumberFormatter
**使用方式**
```
  NSNumber *number = @(12342323.55665);
  NSNumberFormatter *formatter = [[NSNumberFormatter alloc]init];
// 指定货币样式
  formatter.numberStyle = NSNumberFormatterDecimalStyle;
// 自定义样式
  [formatter setPositiveFormat:@"###0.###"];
// 数字之间的分隔符
  [formatter setDecimalSeparator:@":"];
  [formatter setCurrencyDecimalSeparator:@","];
// 前缀符号
  formatter.positivePrefix = @"!";
// 后缀符号
  formatter.positiveSuffix = @"元";
// 间隙中添加的符号
  formatter.paddingCharacter = @"?";
// 格式的宽度
  formatter.formatWidth = 15;
// 最少展示两位小数
  formatter.minimumFractionDigits = 2;
// 最多展示三位整数
  formatter.maximumIntegerDigits = 5;
  NSString *numStr = [formatter stringFromNumber:number];
// ??????!323:557元
```