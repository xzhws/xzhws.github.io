# Git tricks


### git branch 切换分支

``` bash
git branch  #查看本地分支
git branch -r  #查看远程分支（注意切换到对应的文件夹，与远程分支对应）
```
注意不要在本地创建分支 而是在远程创建分支 然后git pull到本地

### git status 查看上次是否有修改

``` bash 
git status         
```
status 具体内容：
```
On branch serial0
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   test_resnet.py

no changes added to commit (use "git add" and/or "git commit -a")
```
### git diff 查看文件具体修改的不同之处

``` bash
git checkout .  #舍弃之前的修改
git checkout  #命令用于切换分支或恢复工作树文件（在checkout某个文件时会覆盖修改的内容 这一点需要注意）
```

### 注释上传新文件

```bash
git add ***.py
git commit -m "message" 
git push #上传被注释的所有文件
```
之后在git的CI上面检查pipline 是否通过

### 提Merge Request流程

1. 首先检查上一步的pipline是否已经完全通过
2. 然后呢在MR的右边把merge的流程assign给mentor

### git CI 分配容器
每次开一个新仓库或者git push 后触发了pipline，需要在gitlab 的setting->member 里面提交容器的申请

### 主仓库的版本更新后，更新本地的代码
当上一个版本的代码更新后
更新本地的master和git仓库的master一致，可以参考方法
https://www.jianshu.com/p/4c1d3b741b19

```bash
git remote -v #查看远程仓库
git fetch origin master:temp #从远程的origin仓库 把代码pull到本地master的tmp分支上面
git diff temp # 查看本地分支 和tmp分支的不同
git merge temp # 将tmp分支与本地的master分支合并
git branch -d temp # 删除一下已经创建的tmp分支
```
