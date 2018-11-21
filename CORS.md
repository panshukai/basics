# 跨域问题
### 本质：浏览器同源策略
### 限制行为：
   + cookie、localstorage、indexDB无法读取
   + dom和js对象无法获取
   + ajax请求无法发送
### 解释：
   + 协议，域名，端口号
### 解决方案：
   1. jsonp跨域
   <br/>原理：img、script等元素标签可以实现跨域
   ```javascript
   //原生的实现方式
   let script = document.createElement('script');
   script.src = 'http://www.nealyang.cn/login?username=Nealyang&callback=callback';
   document.body.appendChild(script);
   function callback(res) {  
     console.log(res);
   }
   ```
   2. iframe + document.domain跨域
   <br/>原理：**同一个主域名下**，通过设置document.domain = 主域名 实现跨域
      + A页面(http:a.example.com/A.html)
         ```html
         <script>
           document.domain = 'example.com';
           let ifr = document.createElement('iframe');
           ifr.src = 'http://sub.example.com/b.html';
           ifr.style.display = 'none';
           document.body.append(ifr);
           ifr.onload = function() {
               let win = ifr.contentWindow;
               alert(win.data);
           }
         </script>
         ```
      + B页面(http:b.example.com/B.html)
         ```html
          <script>
          document.domain = 'example.com';
          window.data = '传送的数据：1111';
          </script>
         ```
   3. location.hash + iframe跨域
   <br/>原理：通过A页面(http://a.exampleA.com/A.html) 嵌套iframe B(http://b.exampleB.com/B.html#data) （传值为data），B页面嵌套iframe C(http://a.exampleA.com/C.html) ，C页面可以获取到A页面并修改data值（A和C为同域），实现A和B的跨域
      + A页面(http://a.exampleA.com/A.html)
      ```html
      <iframe src="http://b.exampleB.com/B.html"></iframe>
      <script>
          document.domain = 'admin.com';
      </script>
      ```
      + B页面(http://b.exampleB.com/B.html)
      ```html
      <script>
          // 设置之后就可获取项目页面中定义的公共资源了
          document.domain = 'admin.com';
      </script>
      ```
   4. window.name + iframe跨域
   <br/>原理：window.name存放于窗口，不会随不同页面加载或者不同域下而变化
      + A页面(http://a.exampleA.com/A.html)
      ```html
      <script>
      var data = null;
      var state = 0;
      var iframe = document.createElement('iframe');
      iframe.src = "http://b.exampleB.com/B.html";
      iframe.style.display = 'none';
      document.body.appendChild(iframe);

      // 第一次加载先加载 b.html，b.html 设置好了 window.name 的值
      // 而后加载 c.html，c.html 的 window.name 的值就是之前 b.html 设置的值
      // 同域的情况下，a.html 可以通过 iframe.contentWindow.name 获取到 b.html 中 windoa.name 的值
      iframe.onload = function() {
          if(state === 0) {
              iframe.src = "http://a.exampleA.com/C.html";
              state = 1;
          }else if(state === 1) {
              data = iframe.contentWindow.name;
              console.log('收到数据:', data);
              document.body.removechild(iframe);//防止一直刷新页面
          }
        }
      </script>
      ```
      + B页面
      ```html
      <script>
          window.name = '这是传递的数据';
      </script>
      ```
  5. H5 postMessage跨域
  <br/>原理：H5新增了postMessage API来实现跨域操作
     + A页面
     ```html
     <script>
        var iframe = document.createElement('iframe');
        iframe.src = "http://b.exampleB.com/B.html";
        iframe.style.display = 'none';
        document.body.appendChild(iframe);
        iframe.onload = function() {
            var targetOrigin = 'http://b.exampleB.com';
            var data = {
              name: '武林外传',
                time: 2005,
                length: 81,
                address: '同福客栈'
            };
            // 向 b.html 发送消息
            window.frames[0].postMessage(data, targetOrigin);

            // 接收 b.html 发送的数据
            window.addEventListener('message', function(e) {
                console.log('b.html 发送来的消息:', e.data);
            })
        }
     </script>
     ```
     + B页面
     ```html
      <script>
      var targetOrigin = 'http://a.exampleA.com';
        window.addEventListener('message', function(e) {
            if(e.source != window.parent) {
                return;
            }
            // 接收 a.html 发送的数据
            console.log('a.html 发送来的消息:', e.data);
            // 向 a.html 发送消息
            parent.postMessage('哈哈，我是b页面，我收到你的消息了', targetOrigin);
        })
      </script>
     ```
  6. CORS(cross-origin resource sharing)
     + 简单请求
        + 条件一：请求方式为head，get，post
        + 条件二：http头信息不超过以下字段:Accept、Accept-Language、Content-Language、 Last-Event-ID、 Content-Type(限于三个值：application/x-www-form-urlencoded、multipart/form-data、text/plain)
     + 非简单请求
        + 条件：请求方式为put，delete或者Content-Type为Application/Json
        + 特点：增加一次“预检”请求，请求方式为option
     + 实现方式：request header中字段origin值在response header的Access-Control-Allow-Origin中(或者为*)来实现跨域
  7. websocket协议跨域
  <br/>原理：HTML5新协议websocket protocol
     + 前端
     ```html
      <div>user input：<input type="text"></div>
      <script src="./socket.io.js"></script>
      <script>
      var socket = io('http://www.domain2.com:8080');

      // 连接成功处理
      socket.on('connect', function() {
          // 监听服务端消息
          socket.on('message', function(msg) {
              console.log('data from server: ---> ' + msg); 
          });

          // 监听服务端关闭
          socket.on('disconnect', function() { 
              console.log('Server socket has closed.'); 
          });
      });

      document.getElementsByTagName('input')[0].onblur = function() {
          socket.send(this.value);
      };
      </script>
     ```
     
     + node server端
     ```javascript
     var http = require('http');var socket = require('socket.io');// 启http服务
     var server = http.createServer(function(req, res) {    
         res.writeHead(200, {        
            'Content-type': 'text/html'
         });    
         res.end();
     });
     server.listen('8080');
     console.log('Server is running at port 8080...');
     // 监听socket连接
     socket.listen(server).on('connection', function(client) {
        // 接收信息    
        client.on('message', function(msg) {        
           client.send('hello：' + msg);        
           console.log('data from client: ---> ' + msg);    
        });    
        // 断开处理    
        client.on('disconnect', function() {        
           console.log('Client socket has closed.');     
        });
     });
     ```
   8. node代理跨域
   <br/>原理：node中间件实现跨域代理，是通过启一个代理服务器，实现数据的转发，也可以通过设置cookieDomainRewrite参数修改响应头中cookie中域名，实现当前域的cookie写入，方便接口登录认证。利用node + express + http-proxy-middleware搭建一个proxy服务器
      + 前端
      ```javascript
         var xhr = new XMLHttpRequest();
         // 前端开关：浏览器是否读写cookie
         xhr.withCredentials = true;
         // 访问http-proxy-middleware代理服务器
         xhr.open('get', 'http://www.domain1.com:3000/login?user=admin', true);
         xhr.send();
      ```
      + node express后端
      ```javascript
      var express = require('express');
      var proxy = require('http-proxy-middleware');
      var app = express();
      app.use('/', proxy({
          // 代理跨域目标接口
          target: 'http://www.domain2.com:8080',
          changeOrigin: true,

          // 修改响应头信息，实现跨域并允许带cookie
          onProxyRes: function(proxyRes, req, res) {
              res.header('Access-Control-Allow-Origin', 'http://www.domain1.com');
              res.header('Access-Control-Allow-Credentials', 'true');
          },

          // 修改响应信息中的cookie域名
          cookieDomainRewrite: 'www.domain1.com'  // 可以为false，表示不修改
      }));

      app.listen(3000);
      console.log('Proxy server is listen at port 3000...');
      ```
   9. nginx代理跨域
