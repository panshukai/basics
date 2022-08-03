## 加载顺序
### 不同页面异步函数同步加载
问：不同页面之间的异步函数同步加载问题<br/>
答：<br/>
```
// main.js
app.config.globalProperties.$onLaunched = new Promise(resolve => {
    app.config.globalProperties.$isResolve = resolve
})
// 第一个异步函数
this.$isResolve();//释放各页面 onLoad 所等待的 Promise;

// 须加载第一个异步函数后执行
await this.$onLaunched;
```
