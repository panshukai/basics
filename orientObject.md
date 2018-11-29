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
<br/>概念：通过构造函数this创建特定接口，new 构造函数来创建子对象
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
   + 缺点：每次调用构造函数来创建新的对象时，都创建了功能相同的函数
   ```javascript
   cat.sayName == dog.sayName //false
   ```
### 原型模式
<br/>概念：通过构造函数来创建特定接口，并将它绑定到原型链上（prototype）
   + 优点：创建每个对象时，他们的特定接口都是复用的
   ```javascript
    function Animal(){}
    Animal.name = 'name';
    Animal.type = 'type';
    Animal.say = function(){
       alert(this.name);
    }
    var cat = new Animal('小猫','cat');
    var dog = new Animal('小🐽','dog');
   ```
   + 缺点：由于特定接口，特定属性都是在原型连上创建的，相当于new的新对象的属性接口都是从原型连上引用过来的,所以当新创建的对象修改了值，就会影响到原型连上，从而影响所有子对象
   ```javascript
   function Person(){};
   Person.prototype = {
       constructor:Person,
       name:"neal",
       friends:['xiaohong','xiaoming'],
       sayName:function(){
           alert(this.name);
       }
   }

   var person1 = new Person();
   var person2 = new Person();

   person1.friends.push('xiaohua');

   alert(person1.friends);//'xiaohong','xiaoming','xiaohua'
   alert(person2.friends);//'xiaohong','xiaoming','xiaohua'
   alert(person1.friends == person2.friends)//true
   ```
### 组合构造函数模式和原型模式
<br/>概念：适用构造函数模式来定义实例对象上的特定接口和属性，使用原型模式来定义不会更改的属性接口
   + 优点：如上概念所述，解决了原型连上的属性接口有可能被更改的缺点
### 动态原型构造模式
<br/>分析：当创建对象时，如果实例对象存在自己的方法就不继承，反之继承原型链上的方法
   ```javascript
   function Person(name,age){
       this.name = name,
       this.age = age,
       if(typeof this.sayName != 'function'){
           Person.prototype.sayName = function(){
               console.log(this.name)
           }
       }
   }
   ```
   
