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













## Generator 生成器