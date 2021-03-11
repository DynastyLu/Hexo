---
title: vuex使用步骤详解
tags: vuex
categories: vue
---
vuex是一个专门为vue.js设计的集中式状态管理架构。状态？我把它理解为在data中的属性需要共享给其他vue组件使用的部分，就叫做状态。简单的说就是data中需要共用的属性。引入Vuex（前提是已经用Vue脚手架工具构建好项目）也完全能够为复杂的单页应用提供驱动。

<!-- more -->

### 一、安装 

1、利用npm包管理工具，进行安装 vuex。在控制命令行中输入下边的命令就可以了。
``` js
npm install vuex --save
```
要注意的是这里一定要加上 –save，因为你这个包我们在生产环境中是要使用的。

2、新建一个store文件夹（这个不是必须的），并在文件夹下新建store.js文件，文件中引入我们的vue和vuex。


``` js
import Vue from 'vue';

import Vuex from 'vuex';
```
3、使用我们vuex，引入之后用Vue.use进行引用。


``` js
import Vue from 'vue';

import Vuex from 'vuex';
```
通过这三步的操作，vuex就算引用成功了，接下来我们就可以尽情的玩耍了。

4、在main.js 中引入新建的vuex文件

``` js
import storeConfig from './vuex/store'
```
5、再然后 , 在实例化 Vue对象时加入 store 对象 :

``` js
new Vue({

 el: '#app',

 router,

 store,//使用store

 template: '<App/>',

 components: { App }

})
``` 
初出茅庐 来个Demo

1、现在我们store.js文件里增加一个常量对象。store.js文件就是我们在引入vuex时的那个文件。

``` js
const state = {

 count:1

}
```
2、用export default 封装代码，让外部可以引用。
``` js
export default new Vuex.Store({

 state

});
``` 
3、新建一个vue的模板，位置在components文件夹下，名字叫count.vue。在模板中我们引入我们刚建的store.js文件，并在模板中用{{$store.state.count}}输出count 的值。
``` js
<template>

 <p>

  <h2>{{msg}}</h2>

  <hr/>

  <h3>{{$store.state.count}}</h3>

 </p>

</template>

<script>

 import store from '@/vuex/store'

 export default{

    data(){

    return{

        msg:'Hello Vuex',

    }

    },

    store

 }

</script>
``` 
4、在store.js文件中加入两个改变state的方法。

``` js
const mutations={

    add(state){

    state.count+=1;

    },

    reduce(state){

    state.count-=1;

    }

}
``` 
这里的mutations是固定的写法，意思是改变的，所以你先不用着急，只知道我们要改变state的数值的方法，必须写在mutations里就可以了。

5、在count.vue模板中加入两个按钮，并调用mutations中的方法。
``` js
<p>

 <button @click="$store.commit('add')">+</button>

 <button @click="$store.commit('reduce')">-</button>

</p>
``` 
这样进行预览就可以实现对vuex中的count进行加减了。

state访问状态对象

const state ，这个就是我们说的访问状态对象，它就是我们SPA（单页应用程序）中的共享值。

学习状态对象赋值给内部对象，也就是把stroe.js中的值，赋值给我们模板里data中的值。有三种赋值方式

一、 通过computed的计算属性直接赋值

computed属性可以在输出前，对data中的值进行改变，我们就利用这种特性把store.js中的state值赋值给我们模板中的data值。

``` js
computed:{

    count(){

    return this.$store.state.count;

    }

}
``` 
这里需要注意的是return this.$store.state.count这一句，一定要写this，要不你会找不到$store的。这种写法很好理解，但是写起来是比较麻烦的，那我们来看看第二种写法。

二、通过mapState的对象来赋值

我们首先要用import引入mapState。

``` js
import {mapState} from 'vuex';
``` 
然后还在computed计算属性里写如下代码：
``` js
computed:mapState({

  count:state=>state.count //理解为传入state对象，修改state.count属性

})
``` 
这里我们使用ES6的箭头函数来给count赋值。

三、通过mapState的数组来赋值

``` js
computed:mapState(["count"])
```
这个算是最简单的写法了，在实际项目开发当中也经常这样使用。


Mutations修改状态（$store.commit( )）

Vuex提供了commit方法来修改状态，我们粘贴出Demo示例代码内容，简单回顾一下，我们在button上的修改方法。
``` js
<button @click="$store.commit('add')">+</button>

<button @click="$store.commit('reduce')">-</button>
``` 
store.js文件：

``` js
const mutations={

 add(state){

  state.count+=1;

 },

 reduce(state){

  state.count-=1;

 }

}
```

传值：这只是一个最简单的修改状态的操作，在实际项目中我们常常需要在修改状态时传值。比如上边的例子，是我们每次只加1，而现在我们要通过所传的值进行相加。其实我们只需要在Mutations里再加上一个参数，并在commit的时候传递就就可以了。我们来看具体代码：

现在store.js文件里给add方法加上一个参数n。
``` js
const mutations={

 add(state,n){

  state.count+=n;

 },

 reduce(state){

  state.count-=1;

 }

}
``` 
在Count.vue里修改按钮的commit( )方法传递的参数，我们传递10，意思就是每次加10.

``` js
<p>

 <button @click="$store.commit('add',10)">+</button>

 <button @click="$store.commit('reduce')">-</button>

</p>
``` 
模板获取Mutations方法

实际开发中我们也不喜欢看到$store.commit( )这样的方法出现，我们希望跟调用模板里的方法一样调用。 
例如：@click=”reduce” 就和没引用vuex插件一样。要达到这种写法，只需要简单的两部就可以了：

1、在模板count.vue里用import 引入我们的mapMutations：

``` js 
import { mapState,mapMutations } from 'vuex';
```
2、在模板的<\script>标签里添加methods属性，并加入mapMutations
``` js
methods:mapMutations([

  'add','reduce'

]),
``` 
通过上边两部，我们已经可以在模板中直接使用我们的reduce或者add方法了，就像下面这样。
``` js
<button @click="reduce">-</button>
``` 
getters计算过滤操作

getters从表面是获得的意思，可以把他看作在获取数据之前进行的一种再编辑,相当于对数据的一个过滤和加工。你可以把它看作store.js的计算属性。

getters基本用法：

比如我们现在要对store.js文件中的count进行一个计算属性的操作，就是在它输出前，给它加上100.我们首先要在store.js里用const声明我们的getters属性。
``` js
const getters = {

 count:function(state){

  return state.count +=100;

 }

}
``` 
写好了gettters之后，我们还需要在Vuex.Store()里引入，由于之前我们已经引入了state和mutations，所以引入里有三个引入属性。代码如下，
``` js
export default new Vuex.Store({

 state,mutations,getters

})
```

在store.js里的配置算是完成了，我们需要到模板页对computed进行配置。在vue 的构造器里边只能有一个computed属性，如果你写多个，只有最后一个computed属性可用，所以要对上节课写的computed属性进行一个改造。改造时我们使用ES6中的展开运算符”…”。
``` js
computed:{

 ...mapState(["count"]),

 count(){

  return this.$store.getters.count;

 }

},
``` 
需要注意的是，你写了这个配置后，在每次count 的值发生变化的时候，都会进行加100的操作。

用mapGetters简化模板写法

我们都知道state和mutations都有map的引用方法把我们模板中的编码进行简化，我们的getters也是有的，我们来看一下代码。

首先用import引入我们的mapGetters
``` js
import { mapState,mapMutations,mapGetters } from 'vuex';
```

在computed属性中加入mapGetters

``` js
...mapGetters(["count"])
``` 
actions异步修改状态

actions和之前讲的Mutations功能基本一样，不同点是，actions是异步的改变state状态，而Mutations是同步改变状态。至于什么是异步什么是同步这里我就不做太多解释了，如果你不懂自己去百度查一下吧。

在store.js中声明actions

actions是可以调用Mutations里的方法的，我们还是继续在上节课的代码基础上进行学习，在actions里调用add和reduce两个方法。
``` js
const actions ={

 addAction(context){

  context.commit('add',10)

 },

 reduceAction({commit}){

  commit('reduce')

 }

}
``` 
在actions里写了两个方法addAction和reduceAction，在方法体里，我们都用commit调用了Mutations里边的方法。细心的小伙伴会发现这两个方法传递的参数也不一样。

•ontext：上下文对象，这里你可以理解称store本身。
•{commit}：直接把commit对象传递过来，可以让方法体逻辑和代码更清晰明了。

模板中的使用

我们需要在count.vue模板中编写代码，让actions生效。我们先复制两个以前有的按钮，并改成我们的actions里的方法名，分别是：addAction和reduceAction。
``` js
<p>

 <button @click="addAction">+</button>

 <button @click="reduceAction">-</button>

</p>
``` 
改造一下我们的methods方法，首先还是用扩展运算符把mapMutations和mapActions加入。

``` js
methods:{

  ...mapMutations([ 

    'add','reduce'

  ]),

  ...mapActions(['addAction','reduceAction'])

},
``` 
你还要记得用import把我们的mapActions引入才可以使用。

增加异步检验

我们现在看的效果和我们用Mutations作的一模一样，肯定有的小伙伴会好奇，那actions有什么用，我们为了演示actions的异步功能，我们增加一个计时器（setTimeOut）延迟执行。在addAction里使用setTimeOut进行延迟执行。
``` js
setTimeOut(()=>{context.commit(reduce)},3000);

console.log('我比reduce提前执行');
``` 

我们可以看到在控制台先打印出了‘我比reduce提前执行'这句话。

module模块组

随着项目的复杂性增加，我们共享的状态越来越多，这时候我们就需要把我们状态的各种操作进行一个分组，分组后再进行按组编写。那今天我们就学习一下module：状态管理器的模块组操作。

声明模块组：

在vuex/store.js中声明模块组，我们还是用我们的const常量的方法声明模块组。代码如下:

``` js
const moduleA={

  state,mutations,getters,actions

}
```

声明好后，我们需要修改原来 Vuex.Stroe里的值：
``` js
export default new Vuex.Store({

  modules:{a:moduleA}

})
``` 
在模板中使用

现在我们要在模板中使用count状态，要用插值的形式写入。

``` js
<h3>{{$store.state.a.count}}</h3>
```

如果想用简单的方法引入，还是要在我们的计算属性中rutrun我们的状态。写法如下：
``` js
computed:{

  count(){

    return this.$store.state.a.count;

  }

}
```