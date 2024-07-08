### Git常用命令大全

##### 设置用户签名

```
git config --global user.name 用户名
git config --global user.email 邮箱
```

##### 初始化本地库

```
git init // 初始化当前目录
git init 目录 //指定目录初始化
```

##### 查看本地库状态

```
git status
```

##### 添加到暂存区

```
git add 文件名
```

##### 提交到本地库

```
git commit -m "提交说明" 文件名
```

##### 查看历史记录

```
git reflog
git log //详细的历史记录
```

##### 版本穿梭

```
git reset --hard 版本号
```

##### 创建分支

```
git branch 分支名
```

##### 查看分支

```
git branch -v
```

##### 切换分支

```
git checkout 分支名
```

##### 合并分支

```
git merge 分支名
```

##### 合并无同祖先分支

```
git merge 远程库链接或别名/分支 --allow-unrelated-histories
git merge git-demo/main --allow-unrelated-histories //例
```

##### 修改分支名

```
git branch -m 旧名字 新名字
```

##### 创建远程库别名

``` 
git remote add 别名 远程库链接
```

##### 查看别名

````
git remote -v
````

##### 推送到远程库

```
git push 别名/远程库链接 分支
```

##### 从远程库拉取

```
git pull 别名/远程库链接 拉取分支
```

##### 远程分支最新更新

```
git fetch 别名/远程库链接 分支
```

##### 克隆到本地

```
git clone 远程库链接 
```

