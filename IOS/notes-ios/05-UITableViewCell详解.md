# UITableViewCell详解

## 初始化
```
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,	// 默认风格，自带标题和一个图片视图，图片在左
    UITableViewCellStyleValue1,		// 只有标题和副标题 副标题在右边
    UITableViewCellStyleValue2,		// 只有标题和副标题，副标题在左边标题的下边
    UITableViewCellStyleSubtitle	// 自带图片视图和主副标题，主副标题都在左边，副标题在下
};
```

## 内部property
```
@property (nonatomic, readonly, retain) UIImageView *imageView;
图片视图，风格允许时才会创建

@property (nonatomic, readonly, retain) UILabel     *textLabel;
标题标签

@property (nonatomic, readonly, retain) UILabel     *detailTextLabel;
副标题标签

@property (nonatomic, readonly, retain) UIView      *contentView;
容纳视图，任何cell的子视图都应该添加在这个上面

@property (nonatomic, retain) UIView                *backgroundView;
背景视图

@property (nonatomic, retain) UIView                *selectedBackgroundView;
选中状态下的背景视图

@property (nonatomic, retain) UIView              *multipleSelectionBackgroundView;
多选选中时的背景视图

@property (nonatomic, readonly, copy) NSString      *reuseIdentifier;
cell的标识符

@property (nonatomic) UITableViewCellSelectionStyle   selectionStyle;
cell被选中时的风格:
typedef NS_ENUM(NSInteger, UITableViewCellSelectionStyle) {
    UITableViewCellSelectionStyleNone,//无
    UITableViewCellSelectionStyleBlue,//蓝色
    UITableViewCellSelectionStyleGray,//灰色
    UITableViewCellSelectionStyleDefault//默认 为蓝色
};

@property (nonatomic, readonly) UITableViewCellEditingStyle editingStyle; 
获取cell的编辑状态，枚举如下:
typedef NS_ENUM(NSInteger, UITableViewCellEditingStyle) {
    UITableViewCellEditingStyleNone,//无编辑
    UITableViewCellEditingStyleDelete,//删除编辑
    UITableViewCellEditingStyleInsert//插入编辑
};

@property (nonatomic, retain) UIView                 *accessoryView;  
附件视图: 添加开关在这添加

@property (nonatomic) UITableViewCellAccessoryType    editingAccessoryType; 
cell编辑时的附件视图风格

@property (nonatomic, retain) UIView                 *editingAccessoryView;  
cell编辑时的附件视图
```

## 设置UITableViewCell中的imageView中image的大小
```
// 修改image的大小
CGSize itemSize = CGSizeMake(40, 40);
UIGraphicsBeginImageContextWithOptions(itemSize,NO,UIScreen.mainScreen.scale);
CGRect imageRect = CGRectMake(0, 0, itemSize.width, itemSize.height);
[cell.imageView.image drawInRect:imageRect];
cell.imageView.image = UIGraphicsGetImageFromCurrentImageContext();
UIGraphicsEndImageContext();
```

## 单元格复用原理


## 参考文章
[reference-属性详解](https://my.oschina.net/u/2340880/blog/410421)

