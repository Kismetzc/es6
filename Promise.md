# Promise

[TOC] 

## 引出Promise

**弊端**：

1 需要自己设计和使用

2 使用第三方库需要查看源码和文档

```js
function requestData(url, successCallback, failtureCallback) {
    // 模拟网络请求

    setTimeout(() => {
        // 拿到请求的结果
        // url传入的是coderwhy, 请求成功

        if (url === "coderwhy") {
            // 成功
            let names = ['abc', 'vba']
            successCallback(names)
        } else { // 否则请求失败
            // 失败
            let errMessage = "请求失败, url错误"
            failtureCallback(errMessage)
        }
    }, 3000);

}


requestData("coderwhy", (res) => {
    console.log(res);
}, (err) => {
    console.log(err);
})

//三秒后，[ 'abc', 'vba' ]


requestData("kobe", (res) => {
    console.log(res);
}, (err) => {
    console.log(err);
})

//三秒后，请求失败, url错误

// 更好的方案，承诺(规范好了所有的代码编写逻辑)
function requestData2() {
    return "承诺"
}

const chengnuo = requestData2()
```



## Promise基本使用

![image-20220427111223532](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220427111223532.png)



```js
// request.js
function requestData(url) {

    // 异步请求代码会被放入executor中
    return new Promise((reslove, reject) => {
        // 模拟网络请求
        setTimeout(() => {
            // 拿到请求的结果
            // url传入的是coderwgy, 请求成功
            if (url == "coderwhy") {
                // 成功
                let names = ["abc", "cba", "nba"]
                reslove(names)
            } else { //否则请求失败
                // 失败
                let errMessage = "请求失败， url错误"
                reject(errMessage)
            }
        }, 3000)
    })
}

// main.js
// const promise = requestData()
// promise.then(() => {
//     console.log("请求成功");
// }).catch(() => {
//     console.log("请求失败");
// })

const promise = requestData()
promise.then((res) => {
    console.log("请求成功:", res);
}, (err) => {
    console.log("第二种请求失败", err);
})

// const promise = requestData()
// output 第二种请求失败 请求失败， url错误

// const promise = requestData("coderhy")
// output 请求成功: [ 'abc', 'cba', 'nba' ]


```

#### 三种状态： pending, fulfilled, rejected

![image-20220429095529024](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220429095529024.png)



```js
// const promise = new Promise((resolve, reject) => {

// })

// promise.then(res => {

// }, err => {

// })


// 等价于下面的代码
// 状态一旦确定下来，那么就是不可更改的(锁定)
new Promise((resolve, reject) => {
    // pending状态: 待定 / 悬而未决的
    console.log("----------");
    // resolve() // 处于fulfilled状态(已敲定，兑现状态)
    reject() // 处于rejected状态（已拒绝）
}).then(res => {
    console.log("res:", res);
}, err => {
    console.log("err:", err);
})
```



## Resolve详解



```js
/* 
 *    resolve(参数)
 *    1> 普通的值或者对象
 *    2> 传入一个Promise
 *        那么当前的Promise状态会由传入的Promise来决定
 *        相当于状态进行了移交
 *    3> 传入一个对象，并且这个对象有实现then方法 (runable/ eatable)
 *       那么也会执行该then方法，并且由该then方法决定后续状态
 */

const newPromise = new Promise((resolve, reject) => {
    // resolve("hhhhh")
    reject("err message")
})
new Promise((resolve, reject) => {
    resolve(newPromise)
}).then(res =>  {
    console.log("res:", res);
}, err => {
    console.log("err", err);
})
```



## then实例方法

![image-20220503104225199](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503104225199.png)



![image-20220503104236994](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503104236994.png)

```js
// Promise 有哪些对象方法

const promise = new Promise((resolve, reject) => {
    resolve("hahaha")
})

// 1 同一个promise可以被多次调用then方法
//当我们的resolve方法被回调时，所有的then方法传入的回调函数都会被调用
promise.then((res) => {
    console.log("res", res);
})

// 2 then方法传入的回调函数 可以有返回值
// then方法本身也是有返回值的，它的返回值是Promise
//  1> 如果我们返回的是一个普通值，那么这个普通的值被作为一个新的Promise的resolve值

// 返回的是普通值
promise.then(res => {
    return "aaaa"
        // return undefined 没有返回值 默认返回undefined
}).then(res => {
    console.log("res::::", res);
    return "bbbbbb"
}).then(res => {
    console.log("res!!!", res);
})

// 2> 如果返回的是一个Promise
promise.then(res => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(1111)
        }, 3000)
    })
}).then(res => {
    console.log("res", res);
})

// 3> 如果返回的是一个对象，并且该对象实现了thenable
promise.then(res => {
    return {
        then: function(resolve, reject) {
            resolve(22222)
        }
    }
}).then(res => {
    console.log(res);
})
```

## catch实例方法

![image-20220503110713023](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503110713023.png)

```js
const promise = new Promise((resolve, reject) => {
    resolve()
        //reject("rejected status")
        // throw new Error("rejected status")
})


// 1. 当executor抛出异常时，也是会调用错误（拒绝）捕获的回调函数的
// promise.then(undefined, (err) => {
//     console.log("err:", err);
// })

// 2. 通过catch方法来传入错误（拒绝）捕获的回调函数
// promise/a+规范
// promise.catch(err => {
//     console.log("err:", err);
// })

// 如果只写一个catch 当作原来的promise(第一个)，如果没有再给返回的新的promise

promise.then(res => {
    // return new Promise((resolve, reject) => {
    //     reject("then rejected status")
    // })
    throw new Error("error message")
}).catch(err => {
    console.log("catch err:", err);
})
```



![](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503110728979.png)

```js
// 3. 拒绝捕获的问题
const promise = new Promise((resolve, reject) => {
    reject("1111")
})

promise.then(res => {

}).catch(err => {
    
})

// promise.catch(err => {

// })
```

```js
// 3. 拒绝捕获的问题
const promise = new Promise((resolve, reject) => {
    //reject("1111")
    resolve()
})

promise.then(res => {

}).then(res => {
    throw new Error("then error message")
}).catch(err => {
    console.log("err: ", err);
})

// promise.catch(err => {

// })
```



```js
// 3. catch方法的返回值
const promise = new Promise((resolve, reject) => {
    reject("1111")
})

// catch方法返回值依然会返回被Promise包裹，
promise.then(err => {
    console.log("err", err);
}).catch(err => {
    console.log("err::", err);
    return "catch return value"
}).then(res => {
    console.log("res result", res);
}).catch(err => {
    console.log("err result", err);
})

//output 
//res result catch return value
```



## finally实例方法

![image-20220503145115523](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503145115523.png)



```js
const promise = new Promise((resolve, reject) => {
    resolve("resolve message")
})

promise.then(res => {
    console.log("res", res);
}).catch(err => {
    console.log("err", err);
}).finally(() => {
    console.log("finally code execute");
})
```



## resolve类方法

![image-20220503150451198](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503150451198.png)

```js
// 转成Promise对象
// function foo() {
//     const obj = { name: "why" }
//     return new Promise((resolve) => {
//         resolve(obj)
//     })
// }

// foo().then(res => {
//     console.log("res:", res);
// })

// 类方法Promise.resolve
// 1.普通的值
// const promise = Promise.resolve({ name: "why" })

// 相当于
// const promise2 = new Promise((resolve, reject) => {
//     resolve({ name: "why" })
// })

// 2.传入Promise
const promise = Promise.resolve(new Promise((resolve, reject) => {
    resolve("1111")
}))

promise.then(res => {
    console.log("res:", res);
})

// 3.传入thenable方法
```



## reject类方法

![image-20220503151042990](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220503151042990.png)

```js
// const promise = Promise.reject("rejected message")
// 相当于
// const promise2 = new Promise((resolve, reject) => {
//     reject("rejected message")
// })

// 注意：reject无论传入什么值都是一样的 返回的都是Promise类型
const promise = Promise.reject({
    then: function(resolve, reject) {
        resolve("111111")
    }
})

promise.then(res => {
    console.log("res", res);
}).catch(err => {
    console.log("err:", err);
})

// output
// err: { then: [Function: then] }
```



## all 方法

![image-20220504105047765](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220504105047765.png)

```js
// 创建多个Promise
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1111)
    }, 1000)
})

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(2222)
    }, 2000)
})

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3333)
    }, 3000)
})

// 需求: 所有的Promise都变成fulfilled时 再拿到结果
// 意外: 在拿到所有结果之前，有有个promise变成rejected，那么整个promise是rejected,返回rejected的结果
Promise.all([p1, p2, p3, "aaaaa"]).then(res => {
    console.log(res);
}).catch(err => {
    console.log("err:", err);
})
```



## allSettled方法

![image-20220504105207442](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220504105207442.png)



```js
// 创建多个Promise
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1111)
    }, 1000)
})

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(2222)
    }, 2000)
})

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3333)
    }, 3000)
})

Promise.allSettled([p1, p2, p3]).then(res => {
    console.log(res);
}).catch(err => {
    console.log(err);
})

// output
/* 
[
  { status: 'fulfilled', value: 1111 },
  { status: 'rejected', reason: 2222 },
  { status: 'fulfilled', value: 3333 }
]
*/
```



## race方法

![image-20220504105906024](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220504105906024.png)

```js
// 创建多个Promise
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1111)
    }, 1000)
})

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(2222)
    }, 200)
})

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3333)
    }, 3000)
})

// race: 竞技/竞赛
// 只要有一个Promise变成fulfilled状态 那么就结束
// 意外: 有先reject 返回reject  //err:  2222
Promise.race([p1, p2, p3]).then(res => {
    console.log("res", res);
}).catch(err => {
    console.log("err: ", err);
})

// output
// res 1111
```



## any方法

![image-20220504110221803](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220504110221803.png)

```js
// 创建多个Promise
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1111)
    }, 1000)
})

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(2222)
    }, 200)
})

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3333)
    }, 3000)
})

// any
// 等到有一个Promise变成fulfilled状态 那么就结束
// 注意与race区别
// 意外：如果全是拒绝 则全部执行完再拒绝 err: AggregateError: All promises were rejected
Promise.any([p1, p2, p3]).then(res => {
    console.log("res", res);
}).catch(err => {
    console.log("err: ", err);
})


// output
// res 1111
```



## *<u>手写Promise(待看)</u>*

### 1 Promise结构设计

### 2 constructor

```js
// ES6 ES2015

const PROMISE_STATUS_PENDING = 'pending'
const PROMISE_STATUS_FULFILLED = 'fulfilled'
const PROMISE_STATUS_REJECTED = 'rejected'

class HYPromise {
    constructor(executor) {
        this.status = PROMISE_STATUS_PENDING
        this.value = undefined
        this.reason = undefined

        const resolve = (value) => {
            if (this.status === PROMISE_STATUS_PENDING) {
                this.status = PROMISE_STATUS_FULFILLED
                this.value = value
                console.log("resolve被调用");
            }

        }

        const reject = (reason) => {
            if (this.status === PROMISE_STATUS_PENDING) {
                this.status = PROMISE_STATUS_REJECTED
                this.reason = reason
                console.log("reject被调用");
            }
        }

        executor(resolve, reject)
    }


}

const promise = new HYPromise((resolve, reject) => {
    console.log("传入的函数被直接调用了");
    resolve(111222)
    reject(3333)
})

promise.then(res => {

}, err => {

})
```



### 3 
