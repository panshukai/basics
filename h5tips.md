## iPhone端弹层穿透问题
### 1、例子：城市级联选择器
### 描述：城市选择器弹出后，弹出层和页面层touchmove相互影响
```vue
//Vue数据变量区域
data(){
/*---------监听函数--------------*/
    handler:function(e){e.preventDefault();},
...
},
<code>//Vue函数方法区域
methods:{
    /*解决iphone页面层级相互影响滑动的问题*/
    closeTouch:function(){
        document.getElementsByTagName("body")[0].addEventListener('touchmove',
            this.handler,{passive:false});//阻止默认事件
        console.log("closeTouch haved happened.");
    },
    openTouch:function(){
        document.getElementsByTagName("body")[0].removeEventListener('touchmove',
            this.handler,{passive:false});//打开默认事件
        console.log("openTouch haved happened.");
    },
    ...
}
//侦听属性
watch:{
    signReasonVisible:function(newvs,oldvs){//picker关闭没有回调函数，所以侦听该属性替代
        if(newvs){
            this.closeTouch();
        }else{
            this.openTouch();
        }
    }
```

### 2、问题描述：在移动端浏览器出现弹层后，手指滑动，整个页面会滚动
解决方案：在弹层的样式上增加  overflow：auto；

## ios端input focus 页面无法弹起
解决方案：input添加blur事件 window.scrollTo(0,0);
## ios端无搜索按钮
解决方案：表单元素外层增加form标签 && action 不为""
## 获取微信登录后带code链接获取参数
```
getUrlQuery: function (name, Url) {
    //URL GET 获取值
    var reg = new RegExp("(^|\\?|&)" + name + "=([^&]*)(\\s|&|$)", "i"),
        url = Url || location.href;
    if (reg.test(url)) return decodeURI(RegExp.$2.replace(/\+/g, " "));
    return "";
},
```
