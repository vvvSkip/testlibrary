> ###### 假设远程源仓库为usernameA，自己fork后的远程仓库为yourusername，自己本地的代码仓库为local



> 问题：pull request的作用
>
> 比如在仓库的主人（A）没有把我们添加为项目合作者的前提下，我们将A的某个仓库名为“a”的仓库clone到自己的电脑中，在自己的电脑进行修改，但是我们会发现我们没办法通过push将代码贡献到B中。
>
>  
>
> 所以要想将你的代码贡献到B中，我们应该：
>
> 1. 在A的仓库中fork项目a （此时我们自己的github就有一个一模一样的仓库a （B），但是URL不同）
> 2. 将我们修改的代码push到自己github中的仓库B中
> 3. 在fork下来的仓库B中New pull request ，主人就会收到请求，并决定要不要接受你的代码
> 4. 也可以可以申请为项目a的contributor，这样可以直接push
>
> (2)  fork了别人的项目到自己的repository之后，别人的项目更新了，我们fork的项目怎么更新？
>
> 答：首先fetch网上的更新到自己的项目上，然后再判断、merge。这里就涉及了下一个问题，pull和fetch有啥区别。
>
>  
>
> （3）fetch+merge与pull效果一样。但是要多用fetch+merge，这样可以检查fetch下来的更新是否合适。pull直接包含了这两步操作，如果你觉得网上的更新没有问题，那直接pull也是可以的。





###### 1. fork项目到本地，原项目仓库地址假设：git@github.com:usernameA/testlibrary.git

![image-20191220095518929](.\images\image-20191220095518929.png)

###### 2. 进入到fork下来的项目中，拷贝git仓库地址，clone项目，假设fork下来的仓库地址为：

git@github.com:yourusername/testlibrary.git

git clone git@github.com:yourusername/testlibrary.git



###### 3. 打开clone下来的项目，运行如下命令：

git remote -v

![image-20191220100306873](.\images\image-20191220100306873.png)

至此，你自己的仓库代码就是别人仓库的最新代码。



###### 4. 添加一个将被同步给 fork 远程的**上游仓库**A，（upstream只是示例名，也可以是其他名称，比如root等等）

```
git remote add upstream git@github.com:usernameA/testlibrary.git
```

再次查看状态确认是否配置成功。

```
git remote -v
```

![image-20191220101329800](.\images\image-20191220101329800.png)



###### 5. 如果要同步别人仓库的分支到本地，可以执行同步fork操作

从上游仓库A fetch 分支和提交点，传送到本地，并会被存储在一个本地分支 upstream/master （默认是master分支）

```
git fetch upstream  //默认会将远程所有的分支fetch下来
```

```
git fetch upstream branchName  //fetch远程指定分支
```

比如：

```
git fetch upstream test
```

![image-20191220102740335](.\images\image-20191220102740335.png)



###### 6. 切换到test分支

```
git checkout -b test upstream/test
```



###### 7. 拉一下最新代码

```
git pull upstream test  // 从远程仓库拉取最新代码
```



> 注意：
>
> 自己的本地分支local的代码一定要是yourname/master上切出来的，因为yourname/master上的代码是远程仓库usernameA上的最新代码，后续开发基于最新代码。



###### 8. 执行合并到upstream操作

使用**git merge upstream/test**命令，把 upstream/test分支合并到本地 test上
 如果想同步远程仓库A非master源的代码到自己仓库B上来，比如说远程远有两个分支，master和dev则合并非master分支的代码, 使用git merge upstream/dev命令，把 upstream/dev 分支合并到本地 dev 上，这样就完成了同步，并且不会丢掉本地修改的内容。



###### 9. push本地代码到自己的远程仓库

执行**git push origin master**将本地仓库master分支的代码推送到自己远程仓库yourusername的master分支上

 或者**git push origin dev:dev**将本地仓库dev分支的代码推送到自己远程仓库yourusername的dev分支上



###### 10. pull和fetch差别

①

```
git fetch origin master:tmp
git diff tmp 
git merge tmp
```

 从远程获取最新的版本到本地的tmp分支上
 之后再进行比较合并

②

```
git pull origin master
```

相当于git fetch + git merge，相当于是从远程获取最新版本并merge到本地，在实际使用中，git fetch更安全一些，因为在merge前，我们可以查看更新情况，然后再决定是否合并



###### 11. 版本回滚

```
git reset --hard HEAD^ 回退到上个版本

git reset --hard HEAD~3 回退到前3次提交之前，以此类推，回退到n次提交之前

git reset --hard commit_id 退到/进到，指定commit的哈希码（这次提交之前或之后的提交都会回滚）
```

回滚后提交可能会失败，必须强制提交

**强推到远程：(可能需要解决对应分支的保护状态)**

```
git push origin HEAD --force
```



###### 12. git reset --hard 放弃本地修改

```
git reset --hard FETCH_HEAD
```

如果想放弃本地的文件修改，可以使用git reset --hard FETCH_HEAD，FETCH_HEAD表示上一次成功git pull之后形成的commit点。然后git pull。

fetch：相当于是从远程获取最新版本到本地，不会自动merge

```
　git fetch orgin master //将远程仓库的master分支下载到本地当前branch中

　git log -p master  ..origin/master //比较本地的master分支和origin/master分支的差别

　git merge origin/master //进行合并
```

git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 **git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。**





[git fork使用](https://www.jianshu.com/p/d73dcee2d907)





