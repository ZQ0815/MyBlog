# UITableView

## 基本使用流程
**tableView的创建**
1.初始化 initWithFrame:style:(plian,gronp)
2.设置属性(行高, 分割线, 表头, 表尾)
3.添加到父视图
4.释放
    
**tableView显示数据**
1.设置dataSource
2.遵循协议`UITableViewDateSource`
3.实现两个必须要实现的方法(a. 返回某个分区行数, b.返回cell)
    
**tableViewCell创建**
1.重用标识符(static初始化一次,节约内存)
2.通过标识符去复用池找cell
3.判断cell是否找到, 如果没有找到, 创建cell(initWithStyle:reuseIdentifier)要释放autorelease
4.判断括号外,对cell进行复制
5.return cell

**UITableViewDelegate代理事件的绑定**
- didSelectRowAtIndexPath

## 项目实际使用
- 如何分组、绑定指定的数据源
- cell复用原理的实现
