# Proxy-Reflect

[TOC] 

## 监听对象的操作

### 监听方式一 defineProperty

![image-20220420100205728](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220420100205728.png)

```js
const obj = {
    name: "why",
    age: 18
}

// Object.defineProperty(obj, "name", {
//     get: function() {
//         console.log("监听到obj对象的name属性被访问了");
//     },
//     set: function() {
//         console.log("监听到obj对象的name属性被设置值");
//     }
// })

// console.log(obj.name);
// obj.name = "kobe"

// output
//监听到obj对象的name属性被访问了
//undefined
//监听到obj对象的name属性被设置值

Object.keys(obj).forEach(key => {
    let value = obj[key]

    Object.defineProperty(obj, key, {
        get: function() {
            console.log(`监听到obj对象的${key}属性被访问了`);
            return value
        },
        set: function(newValue) {
            console.log(`监听到obj对象的${key}属性被设置值`);
            value = newValue
        }
    })
})

obj.name = "kobe"
obj.age = 30

console.log(obj.name);
console.log(obj.age);
```

### 监听对象的方式二

proxy 见下节



## Proxy

![image-20220420100956842](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220420100956842.png)

### 基本使用

```js
const obj = {
    name: "why",
    age: 18
}

const objProxy = new Proxy(obj, {
    
})

console.log(objProxy.name);
console.log(objProxy.age);

objProxy.name = "kobe"
objProxy.age = 30

console.log(obj.name);
console.log(obj.age);
```



### set和get trap捕获器

```js
const obj = {
    name: "why",
    age: 18
}

const objProxy = new Proxy(obj, {
    //获取值时的捕获器
    get: function(target, key) {
        console.log(`监听到对象的${key}属性被访问了`, target);
        return target[key]
    },

    //设置值时的捕获器
    set: function(target, key, newValue) {
        target[key] = newValue
        console.log(`监听到对象的${key}属性被设置值`, target);
    }
})

console.log(objProxy.name);
console.log(objProxy.age);

objProxy.name = "kobe"
objProxy.age = 30

console.log(obj.name);
console.log(obj.age);
```



### in 和 delete 捕获器

```js
const obj = {
    name: "why",
    age: 18
} 

const objProxy = new Proxy(obj, {
    //获取值时的捕获器
    get: function(target, key) {
        console.log(`监听到对象的${key}属性被访问了`, target);
        return target[key]
    },

    //设置值时的捕获器
    set: function(target, key, newValue) {
        target[key] = newValue
        console.log(`监听到对象的${key}属性被设置值`, target);
    },
    // 监听in的捕获器
    has: function(target, key) {
        console.log(`监听到对象的${key}属性in操作`, target);
        return key in target
    },
    // 监听delete的捕获器
    deleteProperty: function(target, key) {
        console.log(`监听到对象的${key}属性in操作`, target);
        delete target[key]
    }
})

// in 操作符
console.log("name" in objProxy);

// delete操作符
delete objProxy.name

console.log(objProxy.name);
```



### 对函数对象的监听

```js
function foo() {

}

const fooProxy = new Proxy(foo, {
    apply: function(target, thisArg, argArray) {
        console.log("对foo函数进行了apply调用");
        return target.apply(thisArg, argArray)
    },
    construct: function(target, argArray, newTarget) {
        console.log("对foo函数进行new调用");
        return new target(...argArray)
    }
})

fooProxy.apply({}, ["as", "fwef"])
new fooProxy("abc", "vba")
```



## Reflect

**它是一个对象**

提供了很多操作JavaScript对象的方法，有点像Object操作对象的方法，替代Object一些方法，Object承受了太多不应该承受的

![image-20220420105603213](C:/Users/zhouq/AppData/Roaming/Typora/typora-user-images/image-20220420105603213.png)



```js
const obj = {
    name: "why",
    age: 18
}

const objProxy = new Proxy(obj, {
    get: function(target, key, receiver) {
        console.log("get-----------");
        return Reflect.get(target, key)
    },
    set: function(target, key, newValue, receiver) {
        console.log("set-----------");
        const result = Reflect.set(target, key, newValue)
        return result
    }
})

objProxy.name = "kobe"
console.log(objProxy.name);
```



### receiver参数的作用

receiver是创建出来的代理对象，改变的this指向

```js
const obj = {
    _name: "why",
    get name() {
        return this._name
    },
    set name(newValue) {
        this._name = newValue
    }
}

const objProxy = new Proxy(obj, {
    get: function(target, key, receiver) {
        // receiver是创建出来的代理对象
        console.log("get--------", key, receiver);
        return Reflect.get(target, key, receiver)
    },
    set: function(target, key, newValue, receiver) {
        console.log("set-------", key);
        Reflect.set(target, key, newValue, receiver)
    }
})

// obj.name = "kobe"
// console.log(objProxy.name);

objProxy.name = "few f"

//	ouput 
//	set------- name
//	set------- _name
```



### construct



```js
function Student(name, age) {
    this.name = name
    this.age = age
}

function Teacher() {

}

// const stu = new Student("why", 18)
// console.log(stu)
// console.log(stu.__proto__ === Student.prototype);
// output
// Student { name: 'why', age: 18 }
// true


//执行Student函数中的内容，但是创建出来对象是Teacher对象
const teacher = Reflect.construct(Student, ["why", 18], Teacher)
console.log(teacher);
console.log(teacher.__proto__ === Teacher.prototype);

//output
//Teacher { name: 'why', age: 18 }
//true
```



