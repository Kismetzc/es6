# this指向

[TOC] 

## 作用

不同对象相同方法  便捷

## this在全局作用域指向window(浏览器)

## 函数里  动态绑定 执行时才确定

this的指向和定义所在的位置无关，和**调用位置和调用方式**有关



### 1 默认绑定

独立函数  指向全局对象

```js
//案例一
function foo1() {
    console.log(this)
}

function foo2() {
    console.log(this)
    foo1() //window
}

function foo3() {
    console.log(this)
    foo2()  //window
}

foo3() //window
```

```js
//案例二
var obj = {
    name: "why"
    foo: function() {
        console.log(this)
    }
}

var bar = obj.foo
bar() //window

```

```js
//案例三
function foo() {
    function bar() {
        console.log(this)
    }
    return bar
}

var fn = foo()
fn() //window
```



### 2 隐式绑定

通过某个对象调用 object.fn()

前提是在调用的对象内部有一个对函数的引用，然后object对象会被js引擎绑定到fn函数的this里面

```js
//案例一

function foo() {
    console.log(this)
}
var obj = {
    name: "why"
    foo: foo
}

obj.foo()
```

```js
//案例二
var obj1 = {
    foo: function() {
        console.log(this.name)
    }
}

var obj2 = {
    name: "obj2",
    bar: obj1.foo
}

obj2.bar() //output obj2
```

### 3 显式绑定

通过call()，apply()和bind()绑定

```js
//案例
function foo() {
    console.log(this)
}

var obj = {
    name: "obj"
}

foo.call(obj)  //obj
foo.apply(obj)  //obj
foo.apply("aaa")  //aaaa
```

#### call和apply区别

```js
function sum(num1, num2, num3) {
	console.log(num1 + num2 + num3, this)
}

sum.call("call",20,30,40)
sum.apply("apply",[20,30,40])
```

#### bind()

```js
function foo() {
    console.log(this)
}

var newFoo = foo.bind("aaa")
newFoo() //aaa 显式绑定优先级高于默认绑定
```



### 4 new绑定

我们通过一个new关键字调用一个函数时(构造器)，这个时候this是在调用这个构造器创建的对象 this = 创建出的对象

```js
function Person(name, age) {
    this.name = name
    this.age = age
}

var p1 = new Person("why", 18)
console.log(p1.name)
var p2 = new Person("james",30)
```



## 绑定优先级

new绑定 >  显式绑定 > 隐式绑定 > 默认绑定

```js
var obj = {
    name: "obj",
    foo: function() {
        console.log(this)
    }
}

var f = new obj.foo() 
f()  //foo，所以new优先级比隐式绑定高
```



## 特殊绑定

### 忽略显式绑定

```js
//apply/call/bind 当传入null/undefined 自动将this绑定成全局对象

function foo() {
    console.log(this);
}

foo.apply("abc")
foo.apply({})

foo.apply(null)  //output window
foo.apply(undefined) //output window

var bar = foo.bind(null)
bar() //output window
```

### 间接函数引用

```js
var obj1 = {
    name: "obj1",
    foo: function() {
        console.log(this)
    }
}

var obj2 = {
    name: "obj2"
}; //;这里很关键  避免和下面的函数变成一个整体

(obj2 bar = obj1.foo)() //这边规定是独立函数调用 output为window
```

### 箭头函数里

new不能和箭头函数一起使用

#### 三种简写

##### 1 只有一个参数省略括号

```js
function foo (a,b,c) {
    console.log(a)
}

 (a,b,c) => {return a}
```

##### 2 只有一行代码省略大括号{},会将执行结果作为返回值

```js
function foo (a) {
    console.log(a)
}

a => console.log(a)
```

##### 3 如果返回值是对象  将函数的{}变为()当作一个整体

```js
var bar = () => ({name: "why", age: 18})
```



#### this规则

箭头函数不遵循this绑定规则而**寻找外层作用域**



### 面试题

 [链接](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA)

#### 1 普通

```js
var name = "window";

var person = {
    name: "person",
    sayName: function() {
        console.log(this.name);
    }
}

function sayName() {
    var sss = person.sayName;
    sss(); //windows : 独立函数调用
    person.sayName(); //person 隐式调用
    (person.sayName)(); //person隐式调用()加不加一样
    (b = person.sayName)(); //window 赋值表达式 间接函数引用
}

sayName();
    
```



#### 2 箭头函数与优先级

注意： 定义对象的时候没有产生作用域，注意箭头函数的外层作用域 以及优先级 

还有**call与apply函数对箭头函数无效**

```js
var name = 'window'

var person1 = {
    name: 'person1',
    foo1: function() {
        console.log(this.name);
    },
    foo2: () => console.log(this.name),
    foo3: function() {
        return function() {
            console.log(this.name);
        }
    },
    foo4: function() {
        return () => {
            console.log(this.name);
        }
    }
}

//person1
person1.foo1(); 
//person2
person1.foo1.call(person2); 
//window 箭头函数
person1.foo2();  
//window 箭头函数
person1.foo2.call(person2);

//window 默认绑定
person1.foo3()(); 
//windows 默认绑定
person1.foo3.call(person2)();
//person2 显式绑定 
person1.foo3().call(person2); 

// person1 先调用foo4里面的this先指向了person1，返回一个箭头函数，最后调用箭头函数此时指向了person1 
person1.foo4()(); 
//person2  第一次调用后箭头函数外层this指向person2 当最后一次调用后变成箭头函数后 this指向外层this 为person2
person1.foo4.call(person2)(); 
//person1 调用先变成箭头函数 call对箭头函数不起作用 所以遵循箭头函数的this指向person1
person1.foo4().call(person2); 
```

这里困扰了好久 原因是没有理清foo和()这种关系 云里雾里 没有明白调用过程中函数内this指向的变化

一般这里的题目分为两次调用  第二次调用要基于第一次调用的结果，例如对比`person1.foo4.call(person2)()`和`person1.foo4().call(person2)`两个表达式仅是调用`foo4`顺序先后的区别。在`前者`中先不调用`foo4`,将`person1.foo4`看作一个函数表达式(一个整体)调用了`call`方法，所以`person1.foo4`被`person2`显示绑定了，这里foo4()里的**this**指向变成`person2`了，在代码中来看就是

```
foo4: function() {
		console.log(this.name) 这里this.name指向person2了
        return () => {
            console.log(this.name)
        }
    }
```

所以在`前者`后一步用()调用时返回一个箭头函数时这里的this向外层找作用域this也指向了person2!!!



#### 3 综合

```js
var name = 'window'

function Person(name) {
    this.name = name
    this.foo1 = function() {
            console.log(this.name)
        },
        this.foo2 = () => console.log(this.name),
        this.foo3 = function() {
            return function() {
                console.log(this.name)
            }
        },
        this.foo4 = function() {
            return () => {
                console.log(this.name)
            }
        }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

//person1
person1.foo1()
//person2 显式绑定优先级大于隐式绑定
person1.foo1.call(person2)

//person1 箭头函数向外找作用域
person1.foo2()
 //person1 call对箭头函数无效
person1.foo2.call(person2)


//windos 返回一个函数默认绑定
person1.foo3()()
//window 这个第一次算错了 call()调用完返回的是函数返回值所以算独立调用一个函数
person1.foo3.call(person2)()
//person2 先调用返回普通函数，然后与perosn2显式绑定 
person1.foo3().call(person2)


//person1 第一次调用完返回的箭头函数，然后调用箭头函数 this指向person1
person1.foo4()()
//person2 先显式绑定person2 然后调用返回一个箭头函数
person1.foo4.call(person2)()
//person1  先调用返回一个箭头函数 this指向person1 然后call对箭头函数无效
person1.foo4().call(person2)
```



#### 4 

```js
var name = 'window'

function Person(name) {
    this.name = name
    this.obj = {
        name: 'obj',
        foo1: function() {
            return function() {
                console.log(this.name)
            }
        },
        foo2: function() {
            return () => {
                console.log(this.name)
            }
        }
    }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

//window 独立函数调用
person1.obj.foo1()()
//window 先显式绑定然后调用返回一个独立函数
person1.obj.foo1.call(person2)()
//person2 先调用函数this指向person1 然后显式绑定person2
person1.obj.foo1().call(person2)

//obj  箭头函数找外层作用域即foo1的作用域 这里找到的是obj!!! 因为被Obj调用
person1.obj.foo2()()
//person2 将显式绑定person2 然后箭头函数找上层作用域 这里经过绑定作用域改为person2了
person1.obj.foo2.call(person2)()
//obj  先调用变成箭头函数  call绑定无效
person1.obj.foo2().call(person2)
```



## 特殊函数

因为这些函数不一定清楚内部的构造 所以就最好记住

### 1 setTimeout()

```js
setTimeout(function() {
    console.log(this) //window
}, 2000)
```



### 2 onclick和addEventListener

```js
const boxDiv = document.querySelector('.box')
boxDiv.onclick = function() {
    console.log(this)  //boxDiv
}
```



### 3 forEach/map/filter/find

```js
var names = ['abc', 'cba', 'nba']
names.forEach(function(item) {
    console.log(this)  //window
})

var names = ['abc', 'cba', 'nba']
names.forEach(function(item) {
    console.log(this)  //ccc
}, 'ccc')

```



完结撒花 没想到单一个this知识就这么多

[MDN文档待看](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/this)

[面试题和详细解释在coderwhy老师微信公众号](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA)
