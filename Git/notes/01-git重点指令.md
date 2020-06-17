# 重点场景指令

## 文件被修改，但是没有执行git add
```
取消指定文件
git checkout filename 
取消所有文件
git checkout .
```

## 所有文件都add了，但是有部分文件不想提交
```
git reset HEAD <filename>
```

## 所有文件都被git commit , 但是再次修改不再产生新的commit
```
git add filename
git commit --amend -m "说明"
```

# 对提交进行操作

## 删除某次提交
```
git rebase -i  HEAD~合并的数量（从目前开始往前数）
把需要删除的提交前面的sqush改为d
然后git status查看有没有冲突，如果有解决冲突并且git add后，执行git rebase --continue（这里会变基，分支会变）
```

## 修改某次提交的信息
```
# 假设你需要修改倒数第n次commit的提交信息
1. git rebase -i HEAD~n

# 进入编辑模式，会出现类似以下的内容
# pick 6608e22 修改代码结构调整导致不能正常显示的问题
# pick 1d381cd 菜单切换可用
# 将需要修改的commit的那一行的pick修改为edit，然后保存退出
# 然后输入
2. git commit --amend

# 进入你需要修改的commit编辑界面，编辑后保存退出
# 修改结束后，输入以下命令返回到最新的commit
3. git rebase --continue

```
