# GIT
## git分支
1. 从父分支上衍生的子分支如果存在未提交的code会报错，这时需要用到 git stash（贮藏）
[for example](https://blog.csdn.net/asheandwine/article/details/79003270)
2. 若当前分支与要被切换的分支无父子关系，则不会报错，可正常切换
3. commit文件到当前分支后，commit的文件在切换分支后被删除😢
   1. 解决方案:git reset --soft HEAD^(回退到上一commit，但保留工作区修改)
## git区间判断
1. git add 表示添加文件到暂存区
2. git commit 表示添加文件到当前分支
3. git status 图解<br/>
![git status](https://segmentfault.com/img/bVbmPeP?w=475&h=256)
