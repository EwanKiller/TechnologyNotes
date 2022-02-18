# 高频工作流

## 提交注释
```
git commit -m "xxxxx"
```
## 推送本地内容
```
git push -u origin <branch name>
```

## 单独提交修改中的某一个文件
```
git add <file name>
git stash -u -k 
git commit -m "xxx"
git push -u origin <branch name>
git stash pop
```

## 修改commit

```
// 修改上一次提交的commit内容
git commit --amend
```

## 撤销 reset

```
// 删除已跟踪的文件，已commit的退回；
git reset 

// 删除未跟踪的文件
git clean 

// 取消指定文件
git reset <fileName> 

// 返回HASH节点，不保留修改
git reset --soft HASH 

// 返回HASH节点，保留修改
git reset --hard HASH 
  
```

## 删除本地修改

```
git checkout -f 
```

# 设置相关

## 检查git版本
```
git vision
```

## 设置用户名和邮箱
```
git config --global user.name "Ewan"
git config --global user.email "houqingchen@hotmai.com"
```

## 初始化git库
```
git init
```

## 建立与远程库的连接
```
git remote add origin <git library path>
```

## 更改拉取代码的方式（https or ssh）

```
git remote set-url origin <project path>
```

## Git 设置代理

```
// 设置代理
git config --global https.proxy http://127.0.0.1:41091

// 取消代理
git config --global --unset https.proxy

// 查看代理
git config --global --get https.proxy
```

## .gitigonre 文件中不忽略 被忽略文件夹里的指定内容
```
1. 不忽略 被忽略文件夹里的指定文件夹
    
node_modules/*
!/node_modules/plz_dont_ignore_this_folder/
    
    
2. 不忽略 被忽略文件夹里的指定文件
    
node_modules/*
!/node_modules/plz_dont_ignore_this_file.js
```

## Git库无法push

    - 可能是由于没有上传SSH，可以在Git库的Settings - Add SSH Key 



# SSH

## macOS生成SSH

```
ssh-keygen
```
- 参考链接
    - https://zhuanlan.zhihu.com/p/99504338

## 检查SSH是否存在
```
cd ~/.SSH
```

# Tag

## 打Tag

```
git tag -a tagName -m "Tag commit" [hashpath]
```

## 推送Tag

```
git push origin tagName
```


# 分支

## 创建分支

```
git checkout -b branchName

or

git branch branchName
git checkout branchName
```

## 切换分支
```
git checkout <branchName>
```
## 查看远程分支

```
git branch -a
```

## 查看远端分支情况

```
git remote -v
```

## 修改远程分支名称
```
// 修改分支名称
git branch -m oldName newName  

// 删除旧分支
git push --delete origin oldName  

// 推送新分支名
git push origin newName  

// 关联本地分支与远程分支
git branch --set-upstream-to origin/newName  
```