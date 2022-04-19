﻿# Promise

[TOC] 

## 先行知识

### 箭头函数

[这个函数讲得很清楚](https://www.bilibili.com/video/BV1Yf4y1t7Aw/?spm_id_from=333.788.recommend_more_video.0)

箭头函数和普通函数的区别
- this指向不同。普通函数this指向调用的，箭头函数指向上一层或者全局

技术蛋老师的总结
![在这里插入图片描述](https://img-blog.csdnimg.cn/c570aa0151df4ec6ac120b47a938465b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASHVtYW4gTGVhcm5pbmc=,size_20,color_FFFFFF,t_70,g_se,x_16)

### JS 回调函数
这篇我个人认为讲得非常小白详细 让我看懂了回调函数
**强烈推荐** 
[从零起步，真正理解Javascript回调函数](https://segmentfault.com/a/1190000021942060)

### JS 异步
[MDN教程 异步JS](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Introducing)
老派callbacks与新派promise



## Promise

### 使用

### 创建

```js
const promiseA = new Promise((resolve, reject) => {
    console.log(11111),
    setTimeout(function() {reslove(123)}, 1000)
}).then(res=>console.log(res))
```

new Promise的时候里面内容会立即执行 如这里的函数参数console和setTimeout会马上执行，所以为了实现调用时执行 Promise一般都是作为函数的返回值

#### pending resolved rejected状态

只要没有throw err .then和.catch返回的都是resovled 的promise  f

反之

### async和await

[推荐看这个视频学习  来自前端小野森森-2](https://www.bilibili.com/video/BV1mb4y1D7hH?spm_id_from=333.999.0.0)

async是async_function，await是操作符

#### async

[async MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)

**async函数一定会返回一个promise对象**。如果一个async函数的返回值看起来不是promise，那么它将会被隐式地包装在一个promise中

```js
//output Promise {<fulfilled>: 1}
async function foo() {
   return 1
}

//the same output
function foo() {
   return Promise.resolve(1)
}
```

#### await

[await MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await)

await只能在异步函数async中使用，另外await 返回 Promise 对象的处理结果

描述：await 表达式会暂停当前 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 的执行，等待 Promise 处理完成。若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，继续执行 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)。

若 Promise 处理异常(rejected)，await 表达式会把 Promise 的异常原因抛出。

另外，**如果 await 操作符后的表达式的值不是一个 Promise，则返回该值本身**



大白话：Promise.then()  成功的情况对应 await  Promise.catch 异常的情况对应try...catch

##### await常见写法

```js
//await 等价于与 Promise.then()
async function test1() {
    const p1 = Promise.resolve(1);
    p1.then(data => {
        console.log('data1',data);
    })
    
    const data = await p1
    console.log('data1',data);
}
test1();  //output data1 1

//If the value is not a Promise, it converts the value to a resolved Promise, and waits for it.
async function test2() {
    const data = await 2
    console.log('data2',data);
}
test2();  //output data2,2


async function test() {
    return 1;
}

async function test3() {
    const data4 = await test(3);
    console.log('data4', data4)
}
test3()  //output data4 1 (1和上面的数字均为number类型)

```

##### try...catch 常见写法

```js
async function test4() {
    const p4 = Promise.reject(4);
    
    try {
        const data4 = await p4;
        console.log('data4', data4);
    } catch (error) {
        console.error('e',error);
    }
}

test4();
```



### 手写Promise

[参考技术蛋老师教学视频](https://www.bilibili.com/video/BV1RR4y1p7my/?spm_id_from=333.788.recommend_more_video.2)

先行知识：

- [bind](https://www.bilibili.com/video/BV1m54y1q7hc?spm_id_from=333.999.0.0)
