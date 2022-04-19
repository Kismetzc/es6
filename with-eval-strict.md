# with-eval-strict

[TOC] 

## with(仅了解)

with语句可以形成自己的作用域

不建议使用with语句

```js
var obj = {name: "why", age:18, message:"obj message"}

function foo() {
    function bar() {
        with(obj) {
            console.log(message) //obj message
            console.log("-----")
        }
    }
}
```



## eval

可以把字符串当作js代码运行

不建议使用

```js
var jsstring = 'var message = "hello world;  console.log(message);"'

eval(jsString)
```



## strict

具有限制性的JavaScript  从而使代码隐式地脱离了“懒散”(sloppy)

限制：

1 通过抛出错误来消除一些原来的静默(slient)错误

2 js引擎执行代码时可以进行更多地优化

3 禁用了在ECMAScript未来版本中可能会定义的一些语法



### 开启strict模式



#### 单个js文件

```js
"use strict"
```



#### 某个函数

```js
function foo() {
    "use strict";
    true.foo = "abc"
} 
```



#### 报错

1. 禁止意外创建全局变量(需要定义)
2. 不允许函数有相同的参数名称
3. 静默错误
4. 不允许使用原先的八进制模式
5. with语句不允许使用
6. eval函数不会向上引用变量



7. 严格模式下的this执行

严格模式下 默认绑定的函数会指向undefined

```js
function foo() {
    console.log(this)
}

var obj = {
    name: "why",
    foo: foo
 

foo()
var obj.foo()
bar() //undefined
```



8. setTimeout()依然指向window

```js
setTimeout(function() {
   console.log(this)
}, 1000)
```

