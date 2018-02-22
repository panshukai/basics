# basics
前端基础笔记

###防抖和节流
```javascript
    var basic = {};
    /*
    func->需要执行性的函数
    time->间隔时间
    */
    basic.debounce = function(func, time) { //防抖
        var last;
        return function() {
            var context = this,
                arg = arguments;
            last = setTimeout(function() {
                func.apply(context, arg);
            }, time);           
        }
    }
    basic.throttle = function(func, time) { //节流
        var last = 0;
        return function() {
            var nextTime = new Date();
            if (nextTime - last > time) {
                func.apply(this, arguments);
                last = nextTime;
            }
        }
    }
    window.onresize=basic.debounce(function(){
    	console.log(0);
    },1000)
```

###继承
```javascript
    /*类式继承 start*/
    function SuperClass() {
        this.superValue = [1];
    }
    SuperClass.prototype.getSuperValue = [2];

    var aa1 = new SuperClass();
    var bb1 = new SuperClass();
    console.log(bb1.getSuperValue); //[2]
    aa1.getSuperValue.push(3); //prototype创建的属性会共享到所有子类
    console.log(bb1.getSuperValue); //[2,3]

    function SubClass() {
        this.subValue = false;
    }
    //继承来自SuperClass的属性方法
    //类的原型对象作用就是为类的原型添加共有方法，但是类不能直接访问这些属性和方法，必须通过原型prototype来访问
    SubClass.prototype = new SuperClass();

    SubClass.prototype.getSubValue = function() {
        return this.subValue;
    }
    var aa = new SubClass();
    var bb = new SubClass();
    //弊端：通过另一个类的prototype原型方法继承父类,会共享其共有属性.导致的结果就是在一个子类中修改了此共有属性，势必影响到其他的子类


    console.log(bb.superValue); //[1]
    console.log(aa.superValue == bb.superValue); //true->类本身的属性方法都被继承到子类的prototype上去了
    aa.superValue.push('html'); //[1,'html']
    console.log(bb.superValue);
    console.log(aa instanceof SuperClass)
    console.log(aa.__proto__ instanceof SuperClass)
    console.log(SubClass.prototype instanceof SuperClass)
    /*类式继承 end*/

    /*构造函数继承 start*/
    function ConstrClass(id) {
        this.nameArr = ['cat'];
        this.id = id; //私有属性初始化为共有属性
    }
    ConstrClass.prototype.showName = function() {
        alert(this.nameArr);
    }

    function SubConstrClass(id) {
        ConstrClass.call(this, id); //通过call和this改变作用域->继承方式  
    }
    var constr1 = new SubConstrClass(1);
    var constr2 = new SubConstrClass(2);
    console.log(constr1.nameArr == constr2.nameArr); //false
    constr1.nameArr.push('dog');
    console.log(constr2.nameArr); //没变是因为this指向了SubConstrClass
    console.log(constr2.showName); //弊端：无法继承原型链上的属性方法
    /*构造函数继承 end*/

    /*组合继承  start*/
    function Combile() {
        this.nameArr = ['cat'];
    }
    Combile.prototype.combileValue = function() {
        return this.nameArr;
    }

    function SubCombile(id) {
        Combile.call(this);
        this.id = id;
    }
    SubCombile.prototype = new Combile();
    SubCombile.prototype.subCombileValue = function() {
        return this.nameArr;
    }
    var subCombile1 = new SubCombile(1);
    console.log(subCombile1.combileValue());
    /*组合继承  end*/

    /*原型继承  start*/
    /*
		@param parent {object} 父对象
		return F {object} 子对象
    */
    function inherit(parent) {
        function F() {}
        F.prototype = parent;
        return new F();
    }
    var book = {
        name: 'book',
        likeBook: ['css book', 'html book']
    };

    var newBook = inherit(book);
    newBook.name = 'ajax book';
    console.log(inherit(book).name); //book
    newBook.likeBook.push('react book');
    console.log(inherit(book).likeBook); //['css book','html book','react book']//父类的共享属性likebook依旧被改变(说明likeBook属性是在object原型链上的)

    /*原型继承  end*/

    /*寄生继承  start*/
    function createBook(obj) {
        //通过原型方式创建新的对象
        var o = inherit(obj);//原型方法属性
        // 拓展新对象
        o.getName = function(name) {
            console.log(name)//this方法属性
        }
        // 返回拓展后的新对象
        return o;
    }
    var jishengObj1 = new createBook(book);
    console.log(jishengObj1); 
    /*寄生继承  end*/

    /*寄生组合继承  start*/
    function inherit(parent) {
        function F() {}
        F.prototype = parent;
        return new F();
    }
	function inheritPrototype(subClass,superClass) {
		// 复制一份父类的原型副本到变量中
		var p = inherit(superClass.prototype);
		// 修正因为重写子类的原型导致子类的constructor属性被修改	
		p.constructor = subClass;
		// 设置子类原型
		subClass.prototype = p;
	}
	//上述函数只继承父类的原型,因此还是会改变原型上的属性
	function Parent(name){
		this.name=name;
		this.nameId=1;
	}
	Parent.prototype.getid=function(){
		return this.nameId;
	}
	function Child(){
		Parent.call(this,name);//继承了父类Parent的属性方法，且子类继承属性方法  不会共享
		this.id=2;
	}
	inheritPrototype(Child,Parent);
	console.log(new Child().__proto__.__proto__.constructor==Parent);
	console.log(new Child().__proto__.constructor==Child);
	console.log(new Child().nameId);//undefined->取不到父类的属性方法(inheritPrototype只复制了原型)
	/*寄生组合继承  end*/

	/*practice*/
	function Animal(name , color){
		this.name = name;
		this.color = color;
		/*一种(构造函数)start*/
		var _this=this;
		this.bind=function(obj,name){
			function F(color){
				this.color=_this.color;
				this.name=name;
			}
			F.prototype=new Animal();
			console.log(F);
			return F;
		}
		/*end*/
	}
	Animal.prototype.say = function(){
		return `I'm a ${this.color} ${this.name}`;
	}
	/*另一种(原型)start*/
	Animal.prototype.bind=function(obj,name){
		function F(color){
			Animal.call(this,color);
			this.name=name;
		}
		F.prototype=new Animal();
		console.log(F);
		return F;
	}
	/*end*/
	const Cat =Animal.bind(null,'cat');
	console.log(Cat);
	const cat = new Cat('white');
	console.log(cat.say());
	if(cat.say() === 'I\'m a white cat' && cat instanceof Cat && cat instanceof Animal){
		console.log('success');
	}
```

###this总结
```javascript
	/*THIS*/
	var count=0;
	//window.count=0;
	function foo1(num){
		//console.log('foo1:'+num);
		this.count++;
		console.log(this == window);//true
	}
	for(var i=0;i<10;i++){
		foo1(i);
	}
	console.log(count);//10

	function foo2() {
	    var a = 2;
	    return function(){
	    	//bar();
	    	console.log( a );
	    }	   
	}
	function bar() {
	    console.log( this.a );
	}
	foo2()();


	function foo3() {
		console.log(this.a);
	}
	var a = 2;
	var o = {
	    a:3,
	    foo:foo3
	}
	var p = {a:4};
	(p.foo = o.foo)();//2->p.foo=o.foo相当于对foo3的引用
	console.log(p.foo = o.foo);

	/*this总结*/
	//es5及以下 this在执行时确定上下文，而不是定义时确定上下文
	
	//函数是否在new中调用，如果是的话this绑定的是新创建的对象
	//var bar = new Foo();

	//函数是否通过call、apply或者其他硬性调用，如果是的话，this绑定的是指定的对象
	//var bar = foo.call(obj);

	//函数是否在某一个上下文对象中调用,如果是的话，this绑定的是那个上下文对象
	//var bar = obj.foo();

	//如果都不是的话，使用默认绑定，如果在严格模式下，就绑定到undefined，注意这里是方法里面的严格声明。否则绑定到全局对象
	//var bar = foo();

	//es6 this在定义时即确定上下文（如下例子）
	function foo4() {
	  // 返回一个arrow function
	    return (a) => {
	    // 这里的`this`是词法上从`foo()`采用
	    	console.log(this);
	        console.log( this.a );
	    };
	    //等同于以下code
	    var _this=this;
	    return function(a){
	    	console.log(_this,_this.a);
	    }
	}
	var obj1 = {
	    a: 2,
	    foo:foo4
	};
	var obj2 = {
	    a: 3
	};
	var bar = foo4.call( obj1 );
	bar.call( obj2 ); // 2, 不是3!
	var aa=obj1.foo().call(obj2);//obj1->es6箭头函数this在定义时确定上下文(此处闭包)
	var a1=foo4();
	a1();//window

		var number=2;
	var obj={
		number:5,
		/*匿名函数自调*/
		fn1:(function(){
				var number;
				this.number*=2;//4
				console.log(this == window);//true->匿名函数自执行作用域 == window
				number=number*2;//undefined
				number=3;//此number在这个匿名函数作用域内
				return function(){
					var num=this.number;
					this.number*=2;
					console.log(num);
					number*=3;
					console.log(number);
				}	
			})(),
		db2:function(){
			this.number*=2;
		}
	}
	var fn1=obj.fn1;
	console.log(number);//4
	fn1();// 4->自执行函数window.number=4(此作用域window) | 4->window.number | 9->闭包返回自执行作用域下的number
	obj.fn1();//5->obj作用域下的number | 27->闭包 返回自执行作用域下的number(上面已经变9，so这里是27)
	console.log(window.number);//8->fn1执行this.number*=2以后变8
	console.log(obj.number);//10->obj.fn1执行this.number*=2以后变10
```

###es6 es7等执行顺序判断
```javascript
	async function async1(){
		console.log('async1 start');//2
		await async2();
		console.log('async1 end');//6
	}
	async function async2(){
		console.log('async2');//3
	}
	console.log('script start');//1
	setTimeout(function(){
		console.log('setTimeout');//8
	},0)
	async1();
	new Promise(function(resolve){
		console.log('promise1');//4
		resolve('promise1');
	}).then(function(){
		console.log('promise2');//7
	});
	console.log('script end');//5

	/*code wrong*/
	/*
	最小改动，使结果输出为
	1
	2
	3
	undefined
	No:1jsCoder
	es6
	.......
	No:2jsCoder
	react
	.......
	No:3jsCoder
	angular
	.......
	*/
	const objWrong={
		name:'jsCoder',
		skill:['es6','react','angular'],
		say:function(){
			for(var i = 0,len=this.skill.length;i<len;i++){					
				setTimeout(function(){
					console.log('No:'+i+this.name);
					console.log(this.skill[i]);
					console.log('.......')
				},0)
				console.log(i);				
			}
		}
	}	
	/*code alter*/
	const objRight={
		name:'jsCoder',
		skill:['es6','react','angular'],
		say:function(){
			for(var i = 1,len=this.skill.length;i<=len;i++){
				var _this=this;
				(function(i){
					new Promise(function(resolve){
						console.log(i);
						resolve();
					}).then(function(){
						setTimeout(function(){
							console.log('No:'+i+_this.name);
							console.log(_this.skill[i-1]);
							console.log('.......')
						},0)
					})
					
				})(i)
				
			}
		}
	}
	objRight.say();
```