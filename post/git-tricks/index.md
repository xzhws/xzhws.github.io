# Git tricks


### git branch 切换分支

``` bash
git branch  #查看本地分支
git branch -r  #查看远程分支（注意切换到对应的文件夹，与远程分支对应）
```
注意不要在本地创建分支，而是在远程创建分支，然后git pull到本地

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

### git删除远程分支的文件或者文件夹
假设本地有：master, main两个分支，远程有main分支  
现在删除远程main分支的文件内容  
参考方法：[https://blog.csdn.net/qq_25623355/article/details/84787784](https://blog.csdn.net/qq_25623355/article/details/84787784)

```bash
git fetch origin main:tmp #把远程的main分支 fetch到本地的tmp分支
git checkout tmp #本地切换到tmp分支
git rm ***.py #删除tmp分支上的***.py文件
git rm -r *** #删除tmp分支上的***文件夹
git commit -m "delete files" #提交修改
git push origin tmp:main #把本地的tmp分支 重新push到远程的main分支上
# 切换到本地的其他分支，并删除刚才建立的tmp分支
git checkout master #本地切换到master分支
git branch -d tmp #删除刚才建立的tmp分支
```

