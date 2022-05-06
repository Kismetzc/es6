# Iterator-Generator

[TOC]

## Iterator 迭代器

### 定义

迭代器是帮助我们对某个数据结构进行遍历的对象

![image-20220505103309733](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220505103309733.png)



```js
// 编写的一个迭代器
const iterator = {
    next: function() {
        return { done: true, value: 123 }
    }
}

// 数组
const names = ["abc", "cba", "nba"]

// 创建一个迭代器对象来访问数组
let index = 0
const namesIterator = {
    next: function() {
        // return { done: false, value: "abc" }
        // return { done: false, value: "cba" }
        // return { done: false, value: "nba" }
        // return { done: true, value: undefined }
        if (index < names.length) {
            return { done: false, value: names[index++] }
        } else {
            return { done: true, value: undefined }
        }
    }
}

console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());

/* 
output

{ done: false, value: 'abc' }
{ done: false, value: 'cba' }
{ done: false, value: 'nba' }
{ done: true, value: undefined }
{ done: true, value: undefined }
{ done: true, value: undefined }
*/
```



```js
const names = ["abc", "cba", "nba"]
const nums = [10, 23, 24, 45]

function createArrayIterator(arr) {
    let index = 0
    return {
        next: function() {
            if (index < arr.length) {
                return { done: false, value: arr[index++] }
            } else {
                return { done: true, value: undefined }
            }
        }
    }
}

const namesIterator = createArrayIterator(names)
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());

const numsIterator = createArrayIterator(nums)
console.log(numsIterator.next());
console.log(numsIterator.next());
console.log(numsIterator.next());
console.log(numsIterator.next());
console.log(numsIterator.next());
console.log(numsIterator.next());
console.log(numsIterator.next());


/* 
output
{ done: false, value: 'abc' }
{ done: false, value: 'cba' }
{ done: false, value: 'nba' }
{ done: true, value: undefined }
{ done: false, value: 10 }
{ done: false, value: 23 }
{ done: false, value: 24 }
{ done: false, value: 45 }
{ done: true, value: undefined }
{ done: true, value: undefined }
{ done: true, value: undefined }
*/

// 创建一个无限的迭代器
function createNumberIterator() {
    let index = 0
    return {
        next: function() {
            return { done: false, value: index++ }
        }
    }
}

const numberIterator = createNumberIterator()
console.log(numberIterator.next());
console.log(numberIterator.next());
console.log(numberIterator.next());
console.log(numberIterator.next());
console.log(numberIterator.next());
console.log(numberIterator.next());

// error 不知道是不是版本问题 TypeError: numberIterator.next is not a function
```



### 可迭代对象

迭代器是一个对象 符合迭代器协议(iterator protocol) 

`const iterator = { next: function() {	return {} }}` 	next函数



可迭代对象是一个对象  符合可迭代协议 (iterable protocol)

`[Symbol.iterator] 函数`

`const iterableObj = { [Symbol.iterator]: function() { return 迭代器 }}`



![image-20220506091047897](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220506091047897.png)



```js
// 数组
const names = ["abc", "cba", "nba"]

// 创建一个迭代器对象来访问数组
let index = 0
const namesIterator = {
    next: function() {
        // return { done: false, value: "abc" }
        // return { done: false, value: "cba" }
        // return { done: false, value: "nba" }
        // return { done: true, value: undefined }

    }
}


const iterableObj = {
    names: ["abc", "cba", "nba"],
    [Symbol.iterator]: function() {
        let index = 0
        return {
            // 注意因为this绑定的关系 next需要写成箭头函数
            next: () => {
                if (index < this.names.length) {
                    return { done: false, value: this.names[index++] }
                } else {
                    return { done: true, value: undefined }
                }
            }
        }
    }
}

// iterableObj 对象是一个可迭代对象
// console.log(iterableObj[Symbol.iterator]);

// 1. 第一次调用iterableObj[Symbol.iterator]函数
// const iterator = iterableObj[Symbol.iterator]()
// console.log(iterator.next());
// console.log(iterator.next());
// console.log(iterator.next());
// console.log(iterator.next());

/* 
{ done: false, value: 'abc' }
{ done: false, value: 'cba' }
{ done: false, value: 'nba' }
{ done: true, value: undefined } */

// 2. 第二次调用
// const iterator1 = iterableObj[Symbol.iterator]()
// console.log(iterator1.next());
// console.log(iterator1.next());
// console.log(iterator1.next());
// console.log(iterator1.next());
/* 
{ done: false, value: 'abc' }
{ done: false, value: 'cba' }
{ done: false, value: 'nba' }
{ done: true, value: undefined }
*/

// 3. for...of可以遍历的东西必须是一个可迭代对象
// const obj = {
//     name: "why",
//     age: 18
// }
// for of为语法糖
for (const item of iterableObj) {
    console.log(item);
}

/* 
abc
cba
nba
*/
```



#### 应用场景

![image-20220506100112210](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220506100112210.png)



```js
const iterableObj = {
    names: ["abc", "cba", "nba"],
    [Symbol.iterator]: function() {
        let index = 0
        return {
            // 注意因为this绑定的关系 next需要写成箭头函数
            next: () => {
                if (index < this.names.length) {
                    return { done: false, value: this.names[index++] }
                } else {
                    return { done: true, value: undefined }
                }
            }
        }
    }
}

const names = ["abc", "cba", "nba"]
const newNames = [...names, ...iterableObj]
console.log(newNames);

const obj = { name: "why", age: 15 }

// ES9(ES2018)中新增的一个特性：
// 针对对象特殊处理的非迭代器，以键值对形式
const newObj = {...obj }
console.log(newObj);

// 3. 解构语法
const [name1, name2] = names //迭代器
const { name, age } = obj // 非迭代器

// 4. 创建一些其它对象时
const set1 = new Set(iterableObj)
const set2 = new Set(names)

const arr1 = Array.from(iterableObj)

// 5. Promise.all
Promise.all(iterableObj).then(res => {
    console.log(res);
})
```

#### 自定义类的可迭代

```js
// 案例： 创建一个教室类，创建出来的对象都是可迭代对象
class Classroom {
    constructor(address, name, students) {
        this.address = address
        this.name = name
        this.students = students
    }

    entry(newStudent) {
        this.students.push(newStudent)
    }

    [Symbol.iterator]() {
        let index = 0
        return {
            next: () => {
                if (index < this.students.length) {
                    return { done: false, value: this.students[index++] }
                } else {
                    return { done: true, value: undefined }
                }
            },
            return: () => {
                console.log("迭代值提前终止了..");
                return { done: true, value: undefined }
            }
        }
    }
}

const classroom = new Classroom("3#309", "机房", ["a", "b", "c"])
classroom.entry("lilei")

for (const stu of classroom) {
    console.log(stu);
    if (stu === 'c') break
}
```



## Generator 生成器

生成器是ES6中新增的一种函数控制，使用的方案，它可以让我们更加灵活地控制函数什么时候继续执行，暂停执行等



![image-20220506101617589](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220506101617589.png)



```js
function* foo() {
    console.log("函数开始执行~");

    const value1 = 100
    console.log("第一段代码开始执行", value1);
    yield

    const value2 = 200
    console.log("第二段代码开始执行", value2);
    yield

    const value3 = 300
    console.log("第三段代码开始执行", value3);
    yield

    console.log("函数执行完毕");

}

// 调用生成器函数时，返回一个生成器对象
const generator = foo()

// 开始执行第一段代码
generator.next()

// 开始执行第二段代码
console.log("-----------");
generator.next()

console.log("-----------");
generator.next()

generator.next()

/* 
函数开始执行~
第一段代码开始执行 100
-----------
第二段代码开始执行 200
-----------
第三段代码开始执行 300
函数执行完毕
*/
```



#### 执行流程

```js
// 当遇到yield是暂停函数的执行
// 当遇到return时候生成器就停止执行

function* foo() {
    console.log("函数开始执行~");

    const value1 = 100
    console.log("第一段代码开始执行", value1);
    yield value1

    const value2 = 200
    console.log("第二段代码开始执行", value2);
    yield

    const value3 = 300
    console.log("第三段代码开始执行", value3);
    yield

    console.log("函数执行完毕");

}

// 调用生成器函数时，返回一个生成器对象
const generator = foo()

// 开始执行第一段代码
console.log("返回值1:", generator.next());
// 开始执行第二段代码
generator.next("返回值2:", generator.next())
generator.next("返回值3:", generator.next())
generator.next("返回值4:", generator.next())

/* 
函数开始执行~
第一段代码开始执行 100
返回值1: { value: 100, done: false }
第二段代码开始执行 200
第三段代码开始执行 300
函数执行完毕
*/
```



### 生成器的其它方法

