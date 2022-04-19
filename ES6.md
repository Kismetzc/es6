# ES6

[TOC] 

## 字面量增强的写法

### 1 property shorthand

```js
var name = "why"
var age = 18

var obj = {
    name,
    age
}
```

### 2 methods shorthand

```js
foo: function() {
    
},
bar() {
    
}
```

### 3 computed property name

```js
var obj = {
    ,
    [name + 123]: "hehehe"
}

obj[name + 123]: "hehehe"
```

## 解构Destructuring

```js
//数组的解构
var names = ['abc', 'cba', 'nba']

//var item1 = names[0]
//var item2 = names[1]
//var item3 = names[2]
var [item1, item2, item3] = names
console.log(item1, item2, item3)

var [, , itemz] = names
console.log(itemz)
```

```js
//对象的解构
var obj = {
    name: "why",
    age: 18,
    height: 1.88
}

var {name, age, height} = obj
console.log(name, age, height)

//解构出的东西重命名
var {name: newName} = obj
//默认值
var { address: newAddress = "Beijing"} = obj
```



## let/const基本使用



```js
var foo = "foo"

let bar = "bar"

const name = "abc"
name = "cba"

//const本质上是传递的值不可修改
//但是如果传递的是一个引用类型(内存地址)，可以通过引用找到相应的对象，去修改对象内部的属性
const obj = {
    foo: "foo"
}

// obj = {}  //不允许
// obj.foo = "aaa"  //允许

//注意事项二  通过let/const定义的变量名是不可以重复定义的

```



### 作用域提升

```js
console.log(foo) //underfined

var foo = "foo"
```

```js
// let/const没有作用域提升
// let/const虽然没有进行作用域提升，但是会在执行上下午阶段被创建出来
console.log(foo)
let foo = "foo"
```



### let/const和window的关系

```js
var foo = "foo"

console.log(window.foo)

let foo = "foo"
```

![image-20220408154359727](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220408154359727.png)



![image-20220408154412831](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220408154412831.png)



### 块级作用域

```js
//ES5 块代码(block code)
{
    //声明一个变量
    var foo = "foo"
    
}
console.log(foo)

//在ES5中只有两个东西会形成作用域
// 1 全局作用域
// 2 函数作用域

function foo() {
    function bar() {
        
    }
}

```

```js
//ES6
// 块级作用域对于let/const/function/class声明的类型是有效的

{
    let foo = "why"
    function demo() {}
    
}
```



**if-swith-for**语句都是块级作用域



### <u>块级作用域的应用场景（待看）</u>





### var let const的选择

var的特殊性：作用域提升，windows全局对象，没有块级作用域



优先使用`const`，需要被重新赋值再改成`let`



## 模板字符串

#### 字符串拼接 ``

```js
const name = "why"
const age = 10

const message = `ma name is ${name}, age is`
console.log(message);

const info = `age double is ${age * 2}`
console.log(info);

function doubleAge() {
    return age * 2
}

const info2 = `double age is ${doubleAge()}`
console.log(info2);
```



#### 标签模板字符串

第一个参数仍然是模块字符串中整个字符串，只是被切成多块，放到了一个数组中

第二个参数是模块字符串中 第一个 ${}

```js
function foo(m, n) {
    console.log(m, n);
}

const name = 'why'
const age = 18

foo `Hello${name}WOrld`


```



## 函数的默认参数

```js
function foo(m = "aaa", n = "bbb") {
    console.log(m, n);
}

foo(0, "")
```

对象参数

```js
//对象参数和默认值以及解构
function printInfo({ name, age } = { name: "why", age: 18 }) {
    console.log(name, age);
}

printInfo({ name: 'kobe', age: 40 })

//另外一种写法
function printInfo1({ name = "why", age = 18 } = {}) {
    console.log(name, age);
}

printInfo1()

// 有默认值的形参最好放到后面
function bar(x, y, z = 30) {
    console.log(x, y, z);
}

bar(100, 200)

//有默认值的函数的length属性
//从默认值开始后面都不算长度
function baz(x, y, z, m = 30, n) {
    console.log(x, y, z, m, n);
}

console.log(baz.length);
```



## 函数的剩余参数

### ![image-20220418102712767](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220418102712767.png)



```js
function foo(m, n, ...args) {
    console.log(m, n);
    console.log(args);

    console.log(arguments);
}

foo(20, 30, 40, 50, 60)

//output
//20 30
//[ 40, 50, 60 ]
//[Arguments] { '0': 20, '1': 30, '2': 40, '3': 50, '4': 60 }
```



### 箭头函数

**箭头函数没有显式原型**

所以不能通过new去调用



**箭头函数没有this**

**箭头函数没有arguments**



### 展开语法 Spread syntax

![image-20220418103507343](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220418103507343.png)

```js
const names = ['abc', "cba", "nba"]
const name = "why"
const info = { name: "why", age: 18 }

//函数调用时
function foo(x, y, z) {
    console.log(x, y, z);
}

// 1 函数调用时
foo(...names)
foo(...name)

// 2 构造数组时 
const newNames = [...names, ...name]
console.log(newNames);

// 3. 构建对象字面量时 ES2018(ES9)
const obj = {...info, address: "Beijing" }
console.log(obj);

// 补充 展开语法实际上是浅拷贝
```

**浅拷贝**

![image-20220418104304225](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220418104304225.png)



## 数值的表示

```js
const num1 = 100 //十进制
// b -> binary
const num2 = 0b100 //二进制
// o -> octonary
const num3 = 0o100 //八进制
// x -> hexodecimal
const num4 = 0x100 //十六进制

console.log(num1, num2, num3, num4);
//output 100 4 64 256

// 大的数值(ES2021 ES12)
const num = 10_000_000_000
console.log(num);
```



## Symbol的基本使用

![image-20220418153118306](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220418153118306.png)



```js
// ES6之前 对象的属性名(key)

// const obj = {
//     name: "why",
//     friend: { name: "kobe" },
//     age: 18
// }


// obj['name'] = "james"

// ES6Symbol的使用
const s1 = Symbol()
const s2 = Symbol()

console.log(s1 === s2)

// ES2019(ES10)中，Symbol还有一个描述
const s3 = Symbol("aaa")
console.log(s3.description);

// 3 Symbol值作为key
// 3.1 在定义对象字面量时使用
const obj = {
    [s1]: "abc",
    [s2]: "cba"
}

// 3.2 新增属性
obj[s3] = "nba"

// 3.3 Object.defineProperty方式
const s4 = Symbol()
Object.defineProperty(obj, s4, {
    enumerable: true,
    configurable: true,
    writable: true,
    value: "mba"
})

console.log(obj[s1], obj[s2], obj[s3], obj[s4]);
// 注意： 不能通过，语法获取
// console.log(obj.s1); //undefined

// 4 使用Symbol作为key的属性名，在遍历/Object.keys等中是获取不到这些symbol值
console.log(Object.keys(obj));
console.log(Object.getOwnPropertyNames(obj));
console.log(Object.getOwnPropertySymbols(obj));

const sKeys = Object.getOwnPropertySymbols(obj)
for (const sKey of sKeys) {
    console.log(obj[sKey]);
}

// 5 Symbol.for(key) / Symbol.keyFor(symbol)
const sa = Symbol.for("aaa")
const sb = Symbol.for("aaa")
console.log(sa === sb);

const key = Symbol.keyFor(sa)
console.log(key);
const sc = Symbol.for(key)
console.log(sa === sc);
```





## Set的基本使用

新增了Set,Map 以及它们的另外形式WeakSet,  WeakMap

类似于数组，但是和数组的区别是元素不能重复

![image-20220418155715489](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220418155715489.png)



基本使用

```js
// 1 创建Set结构
const set = new Set()
set.add(10)
set.add(20)
set.add(40)
set.add(333)

set.add(10);
//2 添加对象时特别注意:
//不同对象
set.add({})
set.add({});
//同一个对象
const obj = {}
set.add(obj)
set.add(obj)

console.log(set);

//3 对数组去重(去除重复的元素)
const arr = [33, 10, 26, 30, 33, 26]
    // const newArr = []
    // for (const item of arr) {
    //     if (newArr.indexOf(item) === -1) {
    //         newArr.push(item)
    //     }
    // }
    // console.log(newArr);


const arrSet = new Set(arr);
// const newArr = Array.from(arrSet);
const newArr = [...arrSet];
console.log(arrSet);
console.log(newArr);

// 4.size属性
console.log(arrSet.size);

// 5.Set方法
arrSet.add(100)
console.log(arrSet);
//delete
arrSet.delete(33)
console.log(arrSet);
//has
console.log(arrSet.has(100));
//clear
// arrSet.clear()
// console.log(arrSet);

// 6.对Set进行遍历
arrSet.forEach(item => {
    console.log(item);
})

for (const item of arrSet) {
    console.log(item);
}
```



### WeakSet

#### 和Set的区别

1. WeakSet只能存放对象类型 不能存放基本数据类型
2. WeakSet对元素的引用是弱引用，如果没有其他引用对某个对象进行引用 那么GC可以对该对象进行回收

```js
const weakSet = new WeakSet()

// 区别一： 只能存放对象类型
// TypeError: Invalid value used in weak set
// weakSet.add(10);

// 区别二： 对对象是一个弱引用
let obj = {
    name: "why",


};

const set = new Set();
//建立的是强引用
set.add(obj);
//建立的是弱引用
weakSet.add(obj);
```

![image-20220419105005246](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220419105005246.png)



**WeakSet不能遍历**



#### 应用场景

```js
// 3. WeakSet应用场景
const personSet = new WeakSet()
class Person {
    constructor() {
        personSet.add(this)
    }

    running() {
        if (!personSet.has(this)) {
            throw new Error("不能通过非构造方法创建出来的对象调用running方法")
        }
        console.log("running", this);
    }
}

const p = new Person();
p.running()

p.running.call({ name: "why" })
```



## Map

![image-20220419160502272](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220419160502272.png)

基本使用

```js
const obj1 = { name: "why" };
const obj2 = { name: "kobe" };

const info = {
    [obj1]: "aaa",
    [obj2]: "bbb"
}

console.log(info);

// 2.Map允许其它数据类型作为key
const map = new Map()
map.set(obj1, "aaa");
map.set(obj2, "bbb");
map.set(1, "ccc");
console.log(map);
// entries 后面es8再讲
const map2 = new Map([
        [obj1, "aaa"],
        [obj2, "bb"],
        [2, "ddd"]
    ])
    // 3. 常见的属性和方法
console.log(map2.size);

// set
map2.set("why", "eee");
console.log(map2);

// get(key)
console.log(map2.get("why"));

// has(key)
console.log(map2.has("why"));

// delete(key)
console.log(map2);


//clear

// 4.遍历map
map2.forEach((item, key) => {
    console.log(item, key);
})
```



### WeakMap

![image-20220419160606006](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220419160606006.png)



#### <u>应用场景</u>（待看）

