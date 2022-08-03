## viewport(vw + vh)
### package.json
```
"devDependencies": {
    "autoprefixer": "^9.4.6",
    "less": "^4.1.2",
    "less-loader": "^7.0.1",
    "postcss": "7.0.32",
    "postcss-px-to-viewport": "^1.1.1",
    "webpack": "^4.46.0"
  },
  "resolutions": {
    "postcss": "7.0.32"
  },
```
### pastcss.config.js
```
'postcss-px-to-viewport': {
    viewportWidth: 750,   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
    // viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
    unitPrecision: 3,     // 指定`px`转换为视窗单位值的小数位数
    viewportUnit: "vw",   //指定需要转换成的视窗单位，建议使用vw
    selectorBlackList: ['.ignore'],// 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
    minPixelValue: 1,     // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
    mediaQuery: false,     // 允许在媒体查询中转换`px`
    landscape: true, // 是否添加根据 landscapeWidth 生成的媒体查询条件 @media (orientation: landscape)
    landscapeUnit: 'vw', // 横屏时使用的单位
    landscapeWidth: 1080 // 横屏时使用的视口宽度
}
```
## css3
### safari浏览器兼容性问题
问：box-shadow在safari 14上不起作用
答： 须作用于块级元素

## http2与1.1有什么区别
#### 传输形式
`1. 1.1基于文本传输`
`2. 2基于二进制数据传输`
