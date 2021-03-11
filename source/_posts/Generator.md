---
title: Generator
tags: Generator
categories: Generator
---
generator是一种函数，这种函数是ES6提出的一种异步编程的解决方案，在它内部，使用yield关键字封装了一个个状态机。这个函数的执行结果，就是一个遍历器对象。

<!-- more -->

### 一、generator函数的理解

    generator函数是ES6提供的一种异步编程的解决方案,generator是一种函数，仍然是普通函数，这种函数是ES6提出的一种异步编程的解决方案，在它内部，使用yield关键字封装了一个个状态机。这个函数的执行结果，就是一个遍历器对象。

### 1、所谓“异步”，就是先执行第一段，转而执行其他的任务，等做好了准备再回来执行后面的;异步加载也叫非阻塞模式加载,浏览器在下载js的同时,还会执行后续的页面处理.

### 2、常见的异步模式有
``` bash
    回调函数
    事件监听
    发布/订阅模式
    Promise
    Generator
    async/await
    后来ES6中，引入了Generator函数；ES7中，async/await更是将异步编程带入了一个全新的阶段。

```

### 二、 Generator函数特征：

``` js
（1）function 关键字和函数之间有一个星号(*),且内部使用yield表达式，定义不同的内部状态。

（2）调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。
```

### 三、定义Gernerator函数

``` bash
function* enerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
}

var x = enerator();

console.log(x.next());// { value: 'hello', done: false }

console.log(x.next());// { value: 'world', done: false }

console.log(x.next());// { value: 'ending', done: true }

console.log(x.next());// { value: 'undefined', done: true }

```

### 遍历器对象的next方法的运行逻辑：

``` js
（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。
```

### 四、yield表达式

### 1、 只能用在generator函数里面，如果要将yield用在循环里，不能用forEach，要用for。因为forEach方法的参数是一个普通函数；

### 2、 yield表达式如果用在另一个表达式中，必须放在()中；

``` bash
    function* demo() {
        
        console.log('Hello' + yield); // SyntaxError  
        
        console.log('Hello' + yield 123); // SyntaxError
        
        console.log('Hello' + (yield)); // OK  
        
        console.log('Hello' + (yield 123)); // OK
        
    }
```

### 3、 yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号。

``` bash 
    function* demo() {
        foo(yield 'a', yield 'b'); // OK  
        let input = yield; // OK
    }
```

### 4、 一个函数只能执行一次return语句，但是可以执行多次yield表达式

### 五、 next方法的参数：

``` bash
    第一次调用a.next()无法输出参数
    每次a.next(赋值)都等于函数体里面yield表达式的值
```

## 示例一：

``` bash

function* foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}
 
var a = foo(5);

a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}
 
var b = foo(5);

b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

上面代码中，第二次运行next方法的时候不带参数，导致 y 的值等于2 * undefined（即NaN），除以 3 以后还是NaN，因此返回对象的value属性也等于NaN。第三次运行Next方法的时候不带参数，所以z等于undefined，返回对象的value属性等于5 + NaN + undefined，即NaN。

如果向next方法提供参数，返回结果就完全不一样了。上面代码第一次调用b的next方法时，返回x+1的值6；第二次调用next方法，将上一次yield表达式的值设为12，因此y等于24，返回y / 3的值8；第三次调用next方法，将上一次yield表达式的值设为13，因此z等于13，这时x等于5，y等于24，所以return语句的值等于42。

注意，由于next方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。V8 引擎直接忽略第一次使用next方法时的参数，只有从第二次使用next方法开始，参数才是有效的。从语义上讲，第一个next方法用来启动遍历器对象，所以不用带有参数。

### 示例二：

``` bash

function* dataConsumer() {
    console.log('Started');
    console.log(`1. ${yield}`);
    console.log(`2. ${yield}`);
    return 'result';
}
 
let genObj = dataConsumer();

genObj.next();

genObj.next('a') // 1. a

genObj.next('b') // 2. b

```

### 六、Generator函数的写法
``` js
    ES6 没有规定，function关键字与函数名之间的星号必须要写在哪个位置，所以以下的写法都能通过。
```

``` bash 
    function * foo(x, y) {}
    function *foo(x, y) {}
    function* foo(x, y) {}
    function*foo(x, y) {}
```

``` bash
$ hexo generate
```


### 七、 Generator.prototype.throw()
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

``` bash
var g = function* () {
    try {
        yield;
    } catch (e) {
        console.log('内部捕获', e);
    }
};

var i = g();
i.next();

try {
    i.throw('a');
    i.throw('b');
} catch (e) {
    console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b

```
上面代码中，遍历器对象i连续抛出两个错误。第一个错误被 Generator 函数体内的catch语句捕获。i第二次抛出错误，由于 Generator 函数内部的catch语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的catch语句捕获。


throw方法可以接受一个参数，该参数会被catch语句接收，建议抛出Error对象的实例。

``` bash
var g = function* () {
    try {
        yield;
    } catch (e) {
        console.log(e);
    }
};

var i = g();
i.next();
i.throw(new Error('出错了！'));
// Error: 出错了！(…)
```

### 八、 Generator.prototype.return() 

Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。

``` bash 

function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }

```

``` bash
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return() // { value: undefined, done: true }
```

### return方法跟next方法的区别:

``` bash 
(1) return终结遍历，之后的yield语句都失效；next返回本次yield语句的返回值。

(2) return没有参数的时候，返回{ value: undefined, done: true }；next没有参数的时候返回本次yield语句的返回值。

(3) return有参数的时候，覆盖本次yield语句的返回值，也就是说，返回{ value: 参数, done: true }；next有参数的时候，覆盖上次yield语句的返回值，返回值可能跟参数有关（参数参与计算的话），也可能跟参数无关（参数不参与计算）。
```