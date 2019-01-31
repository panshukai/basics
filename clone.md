# 深浅拷贝与赋值

## 数据类型
  1. 基本数据类型：undefined，null，number，string，boolean，symbol(直接存放于栈内存中)
  2. 数据类型：object，array等(指针存放于栈，数据存放于堆，指针指向数据)

## 产生影响

 类型 | 改变基本类型 | 改变引用类型 
 :----: | :----: | :----:
 赋值 | 是 | 是 
 浅拷贝 | 否 | 是 
 深拷贝 | 否 | 否 

### 示例
赋值
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
var B = A;
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'B',0
```
浅拷贝
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
function shallow(obj){
	var newObj = {};//假设为对象字面量
	for(var i in obj){
		newObj[i] = obj[i];
	}
	return newObj;
}
var B = shallow(A);
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'A',0
```
深拷贝
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
function shallow(obj){
	if(typeof obj == 'object'){
		var newObj = obj.constructor == Array?[]:{};
		for(var i in obj){
			newObj[i] = typeof obj[i] == 'object'&&obj[i] != null?shallow(obj[i]):obj[i];
		}
		return newObj;
	}	
}
var B = shallow(A);
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'A',1
```

## 深浅拷贝实现方案

### 浅拷贝实现方案

  1. Object.assign
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
Object.assign(B,A);
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'A',0
```

  2. Object.create
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
var B = Object.create(A);
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'A',0
```
  3. Array.slice or Array.concat(适用于array)
```javascript
var A = ['A',{sex:1,age:10}];
var B = A.slice();
B[0] = 'B';
B[1].sex = 0;
console.log(A[0],A[1].sex);//'A',0
```

### 深拷贝实现方案
  1. JSON.parse(JSON.stringify())
```javascript
var A = {name:'A',attr:{sex:1,age:10}};
var B = JSON.parse(JSON.stringify(A));
B.name = 'B';
B.attr.sex = 0;
console.log(A.name,A.attr.sex);//'A',1
```
  2. 如上示例代码
