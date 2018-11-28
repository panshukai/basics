# 面向对象
## 创建对象
### 工厂模式
<br>概念：用函数封装特定接口来创建对象
   + 缺点：不能识别对象来源
   ```javascript
    function createAnimal(name,type){
      var o = new Object();
      o.name = name;
      o.type = type;
      o.sayName = function(){
          alert(this.name)
      }
      return o;
    }
    var cat = createAnimal('小猫','cat');
    var dog = createAnimal('小🐽','dog');
   ```
### 构造函数模式
<br/>概念：通过构造函数创建特定接口，new 构造函数来创建子对象
   + 优点：由于是通过构造函数来执行的，可以解决工厂模式的弊端
   ```javascript
    function Animal(name,type){
        this.name = name;
        this.type = type;
        this.say = function(){
            alert(this.name);
        }
    }

    var cat = new Animal('小猫','cat');
    var dog = new Animal('小🐽','dog');
   ```
