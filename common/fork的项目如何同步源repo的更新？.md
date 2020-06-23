## fork的项目如何同步源repo的更新？

### 1.创建pr

到自己帐号下fork的仓库，点击new pull request创建pr。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200622140850.png)

### 2.点击switching the base

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200623093159.png)

互换源repo和base repo，也可以修改base repo -> 点击 `compare across fork` -> 修改head repo，总之就是互换两个repo的位置，即从往源repo提交变成了从源repo向本地同步。

### 3.提交pr

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200623094650.png)

如图，点击绿色按钮，填写备注，完成同步。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200623094903.png)

最后一步，因为fork的仓库是在你的账号下，所以很显然这次pr需要你来审核合入，在点击create后的页面下拉，点击Merge按钮合入主分支。

![](https://raw.githubusercontent.com/qingshui3000/pic_bed/master/notes/20200623095119.png)

### 4.本地仓库更新

同步后别忘了在本地`git pull`一下，否则后面提交会冲突的。