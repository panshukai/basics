# ES7——async、await
### 一、async
#### async返回一个promise（当return一个value或者throw一个value或一个表达式）
```javascript
async function imAsync(num) {
  if (num > 0) {
    return num // 这里相当于resolve(num)
  } else {
    throw num // 这里相当于reject(num)
  }
}

imAsync(1).then(function (v) {
  console.log(v); // 1
});

// 注意这里是catch
imAsync(0).catch(function (v) {
  console.log(v); // 0
})
```
### 二、await
#### await只能和async一起使用，作用是：如果后面跟着是一个promise，它会阻断当前async的执行，等到后面的resolve执行完后，返回一个resloved value，接着继续执行之后的代码。若await后不是一个promise，则不会阻断
1. code-example-1
```javascript
async function testSync() {
     const response = await new Promise(resolve => {
         setTimeout(() => {
             resolve("async await test...");
          }, 1000);
     });
     console.log(response);
}
testSync();//async await test...
```
2. code-example-2
```javascript
async function asyncAwaitFn(str) {
    return await new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(str)
        }, 1000);
		console.log(0);
    });
	console.log('next')
}
const parallel = async () => { //并行执行
    console.log('parallel1')
    const parallelOne = asyncAwaitFn('string 1');
    const parallelTwo = asyncAwaitFn('string 2')
    //直接打印
    console.log(await parallelOne)
    console.log(await parallelTwo)
    console.log('parallel2')
}
parallel()
```
