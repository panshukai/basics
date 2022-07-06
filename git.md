# GIT
## git分支
1. 从父分支上衍生的子分支如果存在未提交的code会报错，这时需要用到 git stash（贮藏）
[for example](https://blog.csdn.net/asheandwine/article/details/79003270)
2. 若当前分支与要被切换的分支无父子关系，则不会报错，可正常切换
3. commit文件到当前分支后，commit的文件在切换分支后被删除😢
   1. 解决方案:git reset --soft HEAD^(回退到上一commit，但保留工作区修改)
## git合并
1. merge
   - 快速合并
   ```
   git merge feature/a
   ```
   ![图谱](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/833dac4567aa49b09151837efa817736~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)
   - 非快速合并
   ```
   git merge --no-ff feature/a
   ```
   ![图谱](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ec3fe05526fb4f658886475d73c77fd3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

2. 合并
在创建当前分支之后，主分支可能又有新的提交，如下图所示：
![图谱](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36ee866c8f6f47848b226a58d1666b1f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)
   - 合并操作
   ```
   git merge master
   git checkout master
   git merge feature/a
   ```
   ![图谱](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e3d5b0720db94a6e906e20b9604c1711~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)
   - 变基操作
   ```
   git rebase master
   git rebase --continue //如有冲突(生成新的commit号)
   git add . //如有冲突
   git commit -m '' //如有冲突
   git checkout master
   git merge feature/a //如有冲突(merge 新的commit号)
   ```
   ![图谱](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cb9f8722def4956ab5a0c5aff27be25~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)
