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
