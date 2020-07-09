# UITableView的使用详解

## UITableView样式
1. 分组样式（UITableViewStyleGrouped）
2. 不分组（UITableViewStylePlain）

## UITableView介绍
UITableView中每行数据都是一个UITableViewCell，在这个控件中为了显示更多的信息，iOS已经在其内部设置好了多个子控件以供开发者使用。
```
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,    // 左侧显示textLabel（不显示detailTextLabel），imageView可选（显示在最左边）
    UITableViewCellStyleValue1,        // 左侧显示textLabel、右侧显示detailTextLabel（默认蓝色），imageView可选（显示在最左边）
    UITableViewCellStyleValue2,        // 左侧依次显示textLabel(默认蓝色)和detailTextLabel，imageView可选（显示在最左边）
    UITableViewCellStyleSubtitle    // 左上方显示textLabel，左下方显示detailTextLabel（默认灰色）,imageView可选（显示在最左边）
};

// 初始化时进行设置
[[NewsTableViewCell alloc] initWithStyle:UITableViewCellStyleValue1]
```

## UITableView的使用-UITableViewStylePlain
1. 首先在当前UIViewController中创建属性
`@property (nonatomic, strong, readwrite) UITableView *tableView;`
2. 将其添加到当前View中，默认时占整个view的整个页面大小，如果需要设置其大小，需要如何设置。
`[self.view addSubview:_tableView];`
3. 选择UITableView的样式UITableViewStylePlain
`[[UITableView alloc] initWithFrame:self.view.frame style:UITableViewStylePlain]`
4. 设置数据源和代理
```
1. 给用于展示数据源的类添加以上的协议
UIViewController<UITableViewDataSource,UITableViewDelegate>
2. 实现对应的方法
# pragma mark - dataSource

/// 设置单元格的数量
/// @param tableView tableView description
/// @param section section description
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    return 8;
}


/// 设置单元格的样式
/// @param tableView tableView description
/// @param indexPath indexPath description
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    NewsTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"id"];
    if (! cell) {
        cell = [[NewsTableViewCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:@"id"];
    }
    return cell;
}

# pragma mark - Delegate

/// 用于设置单元格的高度
/// @param tableView tableView
/// @param indexPath cellindex
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 50;
}

/// 设置点击单元格之后触发的事件
/// @param tableView tableView
/// @param indexPath cellindex
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    UIViewController *contentViewController = [[UIViewController alloc] init];
    contentViewController.title = @"内容";
    contentViewController.view.backgroundColor = [UIColor whiteColor];
    [self.navigationController pushViewController:contentViewController animated:YES];
}

3. 自定义单元格的样式
@interface NewsTableViewCell : UITableViewCell（继承）
主要实现以下方法
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier {
    self = [super initWithStyle:style reuseIdentifier:reuseIdentifier];
    if (self) {
        [self initSubView];
    }
    return self;
}

#pragma mark 初始化视图
-(void)initSubView{
    // 设置单元格的样式
    self.textLabel.text = @"Table Row";
    self.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
    self.imageView.image = [UIImage imageNamed:@"../icons/icon.png"];
}
```

## UITableView的使用-UITableViewStyleGrouped
1. 首先在当前UIViewController中创建属性
`@property (nonatomic, strong, readwrite) UITableView *tableView;`
2. 将其添加到当前View中，默认时占整个view的整个页面大小，如果需要设置其大小，需要如何设置。
`[self.view addSubview:_tableView];`
3. 选择UITableView的样式UITableViewStylePlain
`[[UITableView alloc] initWithFrame:self.view.frame style:UITableViewStylePlain]`
4. 设置数据源和代理
```
- (instancetype)init {
    self = [super init];
    if (self) {
        self.tabBarItem.title = @"消息-分组";
        self.view.backgroundColor = [UIColor whiteColor];
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    // 往newsView中添加_tableView
    _tableView = [[UITableView alloc] initWithFrame:self.view.frame style:UITableViewStyleGrouped];
    _tableView.dataSource = self;
    _tableView.delegate = self;
    [self.view addSubview:_tableView];
}

- (void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:animated];
    self.tabBarController.navigationItem.title = @"消息-分组";
}

# pragma mark - dataSource

- (NSInteger) numberOfSectionsInTableView:(UITableView *)tableView {
  return 2;
}

/// 设置单元格的数量
/// @param tableView tableView description
/// @param section section description
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    return 8;
}


/// 设置单元格的样式
/// @param tableView tableView description
/// @param indexPath indexPath description
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    NewsGroupTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"id"];
    if (! cell) {
        cell = [[NewsGroupTableViewCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:@"id"];
    }
    return cell;
}

# pragma mark - Delegate

/// 用于设置单元格的高度
/// @param tableView tableView
/// @param indexPath cellindex
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 50;
}

/// 设置点击单元格之后触发的事件
/// @param tableView tableView
/// @param indexPath cellindex
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    UIViewController *contentViewController = [[UIViewController alloc] init];
    contentViewController.title = @"内容";
    contentViewController.view.backgroundColor = [UIColor whiteColor];
    [self.navigationController pushViewController:contentViewController animated:YES];
}
```

5. 设置每个section标题的字段样式
```
/// 设置section头内容，section上方
/// @param tableView tableView description
/// @param section section description
- (nullable NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section {
    NSString *s;
    if ( section == 0 ) {
        s=@"A";
    }else{
        s=@"B";
    }
    return s;
}


/// 设置section头的高度
/// @param tableView tableView description
/// @param section section description
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section{
    return 30;
}


/// 设置section尾内容，section下方
/// @param tableView tableView description
/// @param section section description
- (nullable NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section{
    NSString *s;
    if ( section == 0 ) {
        s=@"A";
    }else{
        s=@"B";
    }
    return s;
}

/// 设置section尾的高度
/// @param tableView tableView description
/// @param section section description
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section{
    return 0;
}


/// 设置section的数量
/// @param tableView tableView description
- (NSInteger) numberOfSectionsInTableView:(UITableView *)tableView {
  return 2;
}
```

## 头部视图和尾部视图

## 常用操作
### 去除cell未满屏幕是多余的分割线
```
_tableView.tableFooterView = [[UIView alloc] init];
```


## 参考文献
[reference-1](https://www.cnblogs.com/gfxxbk/p/5406905.html)