## github如何简单的提交pr(Pull Request)

网上教程帖子很多，这篇文章的步骤很简单，但是我自己提交pr的全过程，可以参考下。

### 1.fork开源项目

在本项目中就是指群主(xzyJavaX)的repo，登录自己的github账号后fork即可。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200622134809.png)

### 2.clone自己fork的repo到本地

fork后在自己的帐号下会生成一个仓库，复制链接后`git clone [url]`到本地。

### 3.配置upstream

clone后会在当前目录生成一个`CoderDay`文件夹，即项目主体，进入该文件夹后首先执行`git remote -v`查看远程仓库，结果如下：

```shell
B000000095775h:CoderDay zhaoxinpeng$ git remote -v
origin	https://github.com/qingshui3000/CoderDay.git (fetch)
origin	https://github.com/qingshui3000/CoderDay.git (push)
```

然后到**群主的repo**复制url执行`git remote add upstream [url] `，再次执行`git remote -v`查看结果如下：

```shell
B000000095775h:CoderDay zhaoxinpeng$ git remote -v
origin	https://github.com/qingshui3000/CoderDay.git (fetch)
origin	https://github.com/qingshui3000/CoderDay.git (push)
upstream	https://github.com/xzyJavaX/CoderDay.git (fetch)
upstream	https://github.com/xzyJavaX/CoderDay.git (push)
```



### 4.在本地修改、提交、上传

本篇文章不涉及到提交到新建分支，直接将更新提交到自己仓库的master分支再发起pr。

文章建议markdown格式(这个应该不用说了)，推荐typora，图片建议使用在线图床，本仓库暂时没有存放图片的打算，更多建议细则请查阅根规范和准则文档。

### 5.发起pr

在add、commit、push后，打开浏览器到自己的仓库页面，刷新确保更新提交上了，点击new pull request，

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200622140850.png)

然后

### 6.开源项目管理者审核通过pr

### 7.如何解决冲突

todo。。。



