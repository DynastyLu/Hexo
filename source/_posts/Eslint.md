---
title: ESLint
tags: ESLint
categories: ESLint
---
ESLint是一个用来识别 ECMAScript 并且按照规则给出报告的代码检测工具，使用它可以避免低级错误和统一代码的风格。ESLint被设计为完全可配置的，主要有两种方式来配置ESLint：

### 一、 为什么要使用ESlint？

有的时候多人开发，代码的风格，用的代码编辑器都各不相同，所以为了大家能保持一种统一的风格，ESLint就可以帮我们检查代码的格式，和风格。

<!-- more -->

### 二、 简介

``` js 
通过用 ESLint 来检查一些规则，我们可以：

统一代码风格规则，如：代码缩进用几个空格；是否用驼峰命名法来命名变量和函数名等。

减少错误， 如：相等比较必须用 === ，变量在使用前必须被声明，在条件语句中不能使用赋值语句等。

提高代码质量，如：函数最多有多少条件分支；最多有几个参数，代码块最多能嵌套多少层等

```

### 给React项目添加ESLint

### 三、 安装

``` bash
    npm --save-dev install eslint
```

``` bash
    npm install  --save-dev eslint eslint-loader
```

### 四、 webpack配置中使用eslint加载器

webpack.js中

``` bash
module: {
    loaders: [
        {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'react-hot!babel'
        },
        {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'eslint-loader'
        }
    ]
}
```

我们既可以在webpack配置文件中指定检测规则，也可以遵循最佳实践在一个专门的文件中指定检测规则。

新建 .eslintrc文件, 我们可以在该文件中指定规则

``` bash 
{
    "rules": {
        "max-len": [1, 120, 2, {ignoreComments: true}],
        "prop-types": [2]
    }
}
```

首先我们要在Webpack配置文件中引入该文件。

webpack.cofnig.js

``` bash 

devServer: {
    contentBase: './dist',
    hot: true,
    historyApiFallback: true
},
eslint: {
    configFile: './.eslintrc'
}

```

### 扩展ESLint规则

安装依赖

``` bash 
    npm --save-dev install eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y
```

接下来，通过一行代码的配置来让我们可以使用Airbnb的ESLint配置(你可以通过查看node_modules里面的包来查看，这个配置包含了jsx和React的规则)

``` bash 
{
    parser: "babel-eslint",
    "extends": "airbnb",
    "rules": {
        "max-len": [1, 120, 2, {ignoreComments: true}],
        "prop-types": [2]
    }
}
```
我们可以看到可以很简单的使用别人的配置规则来扩展ESLint规则。我们还可以使用其他的扩展，但目前Airbnb代码规范和ESlint配置非常的受欢迎并被大多数开发者所接受。

### 给vue项目添加ESLint

### eslint配置方式有两种：

注释配置：使用js注释来直接嵌入ESLint配置信息到一个文件里
配置文件：使用一个js，JSON或者YAML文件来给整个目录和它的子目录指定配置信息。这些配置可以写在一个文件名为.eslintrc.*的文件或者在package.json文件里的eslintConfig项里，这两种方式ESLint都会自动寻找然后读取，或者你也可以在命令行里指定一个配置文件。
有几种东西是可以配置的：

``` bash 
    1、 环境：你的脚本会在哪种环境下运行。每个环境带来了一组特定的预定义的全局变量。
   
    2、 全局变量：脚本运行期间会访问额外的全局变量。
   
    3、 规则：使用那些规则，并且规则的等级是多少。

```

.eslintrc

``` bash 
module.exports = {
    root: true,
    parser: 'babel-eslint',//解析器，这里我们使用babel-eslint
    parserOptions: {
        sourceType: 'module'//类型为module，因为代码使用了使用了ECMAScript模块
    },
    env: {
        browser: true,//预定义的全局变量，这里是浏览器环境
    },
    // https://github.com/feross/standard/blob/master/RULES.md#javascript-standard-style
    //extends: 'standard', //扩展，可以通过字符串或者一个数组来扩展规则
    // required to lint *.vue files
    plugins: [
    'html' //插件，此插件用于识别文件中的js代码，没有MIME类型标识没有script标签也可以识别到，因此拿来识别.vue文件中的js代码
    ],
    // add your custom rules here
    'rules': {
        //这里写自定义规则
    }
}
```

### ESLint的规则有三种级别：

``` bash 
    • off 或 0：表示不验证规则。
    • warn 或 1：表示验证规则，当不满足时，给警告。
    • error或 2 ：表示验证规则，不满足时报错。
```

有时候代码里有些特殊情况需要我们在某一行或者某几行关闭ESLint检测，可以使用注释：
``` bash 
    /* eslint-disable */

    alert('foo');

    /* eslint-enable */
```

下面的代码会关闭某一行的所有规则

``` bash
    alert('foo'); // eslint-disable-line

    // eslint-disable-next-line

    alert('foo');
```

下面的代码在某一行关闭指定的规则

``` bash 
    alert('foo'); // eslint-disable-line no-alert

    // eslint-disable-next-line no-alert

    alert('foo');
```

### 常见规则

``` bash
•	indent： 代码缩进。参数有
•	no-mixed-spaces-and-tabs： 代码缩进不能混用空格和tab。
•	no-mixed-spaces-and-tabs： 代码缩进不能混用空格和tab。
•	camelcase： 变量，函数名遵循驼峰命名法。
•	quotes： 字符串的引号。
•	curly： 在 if，else if，else或 while 的代码块中，即使只有一行代码，也要用写在{} 中。
•	eqeqeq： 比较用 ===或 !==。
•	no-cond-assign： 不在 if 中使用赋值操作。
•	no-undef： 变量和函数在使用前必须先声明。全局变量或函数除外。
•	no-unused-vars：变量定义后会一定要被使用。
•	no-alert： 代码不用 alert,confirm 和 prompt。系统的弹出框比较丑，一般都用自定义的弹出框。
•	max-params： 函数最多有几个参数。默认是3个。
•	max-statements： 函数最多有多少条语句。
•	max-depth：代码块中默认嵌套深度。
```

### eslint 的默认推荐规则

``` bash
"extends": "eslint:recommended",// 启用 eslint 的默认推荐规
"rules": {
    // 新增的一些规则
    "indent": ["error", 4],
    "linebreak-style": ["error", "unix"],
    "quotes": ["error", "double"],
    "semi": ["error", "always"],

    // 覆盖一些规则的配置
    "comma-dangle": ["error", "always"],
    "no-cond-assign": ["error", "always"],

    // 禁用一些规则
    "no-console": "off",
}

```

### 常见规则错误

``` bash
    "no-alert": 0,//禁止使用alert confirm prompt
    "no-array-constructor": 2,//禁止使用数组构造器
    "no-bitwise": 0,//禁止使用按位运算符
    "no-caller": 1,//禁止使用arguments.caller或arguments.callee
    "no-catch-shadow": 2,//禁止catch子句参数与外部作用域变量同名
    "no-class-assign": 2,//禁止给类赋值
    "no-cond-assign": 2,//禁止在条件表达式中使用赋值语句
    "no-console": 2,//禁止使用console
    "no-const-assign": 2,//禁止修改const声明的变量
    "no-constant-condition": 2,//禁止在条件中使用常量表达式 if(true) if(1)
    "no-continue": 0,//禁止使用continue
    "no-control-regex": 2,//禁止在正则表达式中使用控制字符
    "no-debugger": 2,//禁止使用debugger
    "no-delete-var": 2,//不能对var声明的变量使用delete操作符
    "no-div-regex": 1,//不能使用看起来像除法的正则表达式/=foo/
    "no-dupe-keys": 2,//在创建对象字面量时不允许键重复 {a:1,a:1}
    "no-dupe-args": 2,//函数参数不能重复
    "no-duplicate-case": 2,//switch中的case标签不能重复
    "no-else-return": 2,//如果if语句里面有return,后面不能跟else语句
    "no-empty": 2,//块语句中的内容不能为空
    "no-empty-character-class": 2,//正则表达式中的[]内容不能为空
    "no-empty-label": 2,//禁止使用空label
    "no-eq-null": 2,//禁止对null使用==或!=运算符
    "no-eval": 1,//禁止使用eval
    "no-ex-assign": 2,//禁止给catch语句中的异常参数赋值
    "no-extend-native": 2,//禁止扩展native对象
    "no-extra-bind": 2,//禁止不必要的函数绑定
    "no-extra-boolean-cast": 2,//禁止不必要的bool转换
    "no-extra-parens": 2,//禁止非必要的括号
    "no-extra-semi": 2,//禁止多余的冒号
    "no-fallthrough": 1,//禁止switch穿透
    "no-floating-decimal": 2,//禁止省略浮点数中的0 .5 3.
    "no-func-assign": 2,//禁止重复的函数声明
    "no-implicit-coercion": 1,//禁止隐式转换
    "no-implied-eval": 2,//禁止使用隐式eval
    "no-inline-comments": 0,//禁止行内备注
    "no-inner-declarations": [2, "functions"],//禁止在块语句中使用声明（变量或函数）
    "no-invalid-regexp": 2,//禁止无效的正则表达式
    "no-invalid-this": 2,//禁止无效的this，只能用在构造器，类，对象字面量
    "no-irregular-whitespace": 2,//不能有不规则的空格
    "no-iterator": 2,//禁止使用__iterator__ 属性
    "no-label-var": 2,//label名不能与var声明的变量名相同
    "no-labels": 2,//禁止标签声明
    "no-lone-blocks": 2,//禁止不必要的嵌套块
    "no-lonely-if": 2,//禁止else语句内只有if语句
    "no-loop-func": 1,//禁止在循环中使用函数（如果没有引用外部变量不形成闭包就可以）
    "no-mixed-requires": [0, false],//声明时不能混用声明类型
    "no-mixed-spaces-and-tabs": [2, false],//禁止混用tab和空格
    "linebreak-style": [0, "windows"],//换行风格
    "no-multi-spaces": 1,//不能用多余的空格
    "no-multi-str": 2,//字符串不能用\换行
    "no-multiple-empty-lines": [1, {"max": 2}],//空行最多不能超过2行
    "no-native-reassign": 2,//不能重写native对象
    "no-negated-in-lhs": 2,//in 操作符的左边不能有!
    "no-nested-ternary": 0,//禁止使用嵌套的三目运算
    "no-new": 1,//禁止在使用new构造一个实例后不赋值
    "no-new-func": 1,//禁止使用new Function
    "no-new-object": 2,//禁止使用new Object()
    "no-new-require": 2,//禁止使用new require
    "no-new-wrappers": 2,//禁止使用new创建包装实例，new String new Boolean new Number
    "no-obj-calls": 2,//不能调用内置的全局对象，比如Math() JSON()
    "no-octal": 2,//禁止使用八进制数字
    "no-octal-escape": 2,//禁止使用八进制转义序列
    "no-param-reassign": 2,//禁止给参数重新赋值
    "no-path-concat": 0,//node中不能使用__dirname或__filename做路径拼接
    "no-plusplus": 0,//禁止使用++，--
    "no-process-env": 0,//禁止使用process.env
    "no-process-exit": 0,//禁止使用process.exit()
    "no-proto": 2,//禁止使用__proto__属性
    "no-redeclare": 2,//禁止重复声明变量
    "no-regex-spaces": 2,//禁止在正则表达式字面量中使用多个空格 /foo bar/
    "no-restricted-modules": 0,//如果禁用了指定模块，使用就会报错
    "no-return-assign": 1,//return 语句中不能有赋值表达式
    "no-script-url": 0,//禁止使用javascript:void(0)
    "no-self-compare": 2,//不能比较自身
    "no-sequences": 0,//禁止使用逗号运算符
    "no-shadow": 2,//外部作用域中的变量不能与它所包含的作用域中的变量或参数同名
    "no-shadow-restricted-names": 2,//严格模式中规定的限制标识符不能作为声明时的变量名使用
    "no-spaced-func": 2,//函数调用时 函数名与()之间不能有空格
    "no-sparse-arrays": 2,//禁止稀疏数组， [1,,2]
    "no-sync": 0,//nodejs 禁止同步方法
    "no-ternary": 0,//禁止使用三目运算符
    "no-trailing-spaces": 1,//一行结束后面不要有空格
    "no-this-before-super": 0,//在调用super()之前不能使用this或super
    "no-throw-literal": 2,//禁止抛出字面量错误 throw "error";
    "no-undef": 1,//不能有未定义的变量
    "no-undef-init": 2,//变量初始化时不能直接给它赋值为undefined
    "no-undefined": 2,//不能使用undefined
    "no-unexpected-multiline": 2,//避免多行表达式
    "no-underscore-dangle": 1,//标识符不能以_开头或结尾
    "no-unneeded-ternary": 2,//禁止不必要的嵌套 var isYes = answer === 1 ? true : false;
    "no-unreachable": 2,//不能有无法执行的代码
    "no-unused-expressions": 2,//禁止无用的表达式
    "no-unused-vars": [2, {"vars": "all", "args": "after-used"}],//不能有声明后未被使用的变量或参数
    "no-use-before-define": 2,//未定义前不能使用
    "no-useless-call": 2,//禁止不必要的call和apply
    "no-void": 2,//禁用void操作符
    "no-var": 0,//禁用var，用let和const代替
    "no-warning-comments": [1, { "terms": ["todo", "fixme", "xxx"], "location": "start" }],//不能有警告备注
    "no-with": 2,//禁用with
    "array-bracket-spacing": [2, "never"],//是否允许非空数组里面有多余的空格
    "arrow-parens": 0,//箭头函数用小括号括起来
    "arrow-spacing": 0,//=>的前/后括号
    "accessor-pairs": 0,//在对象中使用getter/setter
    "block-scoped-var": 0,//块语句中使用var
    "brace-style": [1, "1tbs"],//大括号风格
    "callback-return": 1,//避免多次调用回调什么的
    "camelcase": 2,//强制驼峰法命名
    "comma-dangle": [2, "never"],//对象字面量项尾不能有逗号
    "comma-spacing": 0,//逗号前后的空格
    "comma-style": [2, "last"],//逗号风格，换行时在行首还是行尾
    "complexity": [0, 11],//循环复杂度
    "computed-property-spacing": [0, "never"],//是否允许计算后的键名什么的
    "consistent-return": 0,//return 后面是否允许省略
    "consistent-this": [2, "that"],//this别名
    "constructor-super": 0,//非派生类不能调用super，派生类必须调用super
    "curly": [2, "all"],//必须使用 if(){} 中的{}
    "default-case": 2,//switch语句最后必须有default
    "dot-location": 0,//对象访问符的位置，换行的时候在行首还是行尾
    "dot-notation": [0, { "allowKeywords": true }],//避免不必要的方括号
    "eol-last": 0,//文件以单一的换行符结束
    "eqeqeq": 2,//必须使用全等
    "func-names": 0,//函数表达式必须有名字
    "func-style": [0, "declaration"],//函数风格，规定只能使用函数声明/函数表达式
    "generator-star-spacing": 0,//生成器函数*的前后空格
    "guard-for-in": 0,//for in循环要用if语句过滤
    "handle-callback-err": 0,//nodejs 处理错误
    "id-length": 0,//变量名长度
    "indent": [2, 4],//缩进风格
    "init-declarations": 0,//声明时必须赋初值
    "key-spacing": [0, { "beforeColon": false, "afterColon": true }],//对象字面量中冒号的前后空格
    "lines-around-comment": 0,//行前/行后备注
    "max-depth": [0, 4],//嵌套块深度
    "max-len": [0, 80, 4],//字符串最大长度
    "max-nested-callbacks": [0, 2],//回调嵌套深度
    "max-params": [0, 3],//函数最多只能有3个参数
    "max-statements": [0, 10],//函数内最多有几个声明
    "new-cap": 2,//函数名首行大写必须使用new方式调用，首行小写必须用不带new方式调用
    "new-parens": 2,//new时必须加小括号
    "newline-after-var": 2,//变量声明后是否需要空一行
    "object-curly-spacing": [0, "never"],//大括号内是否允许不必要的空格
    "object-shorthand": 0,//强制对象字面量缩写语法
    "one-var": 1,//连续声明
    "operator-assignment": [0, "always"],//赋值运算符 += -=什么的
    "operator-linebreak": [2, "after"],//换行时运算符在行尾还是行首
    "padded-blocks": 0,//块语句内行首行尾是否要空行
    "prefer-const": 0,//首选const
    "prefer-spread": 0,//首选展开运算
    "prefer-reflect": 0,//首选Reflect的方法
    "quotes": [1, "single"],//引号类型 "" ''
    "quote-props":[2, "always"],//对象字面量中的属性名是否强制双引号
    "radix": 2,//parseInt必须指定第二个参数
    "id-match": 0,//命名检测
    "require-yield": 0,//生成器函数必须有yield
    "semi": [2, "always"],//语句强制分号结尾
    "semi-spacing": [0, {"before": false, "after": true}],//分号前后空格
    "sort-vars": 0,//变量声明时排序
    "space-after-keywords": [0, "always"],//关键字后面是否要空一格
    "space-before-blocks": [0, "always"],//不以新行开始的块{前面要不要有空格
    "space-before-function-paren": [0, "always"],//函数定义时括号前面要不要有空格
    "space-in-parens": [0, "never"],//小括号里面要不要有空格
    "space-infix-ops": 0,//中缀操作符周围要不要有空格
    "space-return-throw-case": 2,//return throw case后面要不要加空格
    "space-unary-ops": [0, { "words": true, "nonwords": false }],//一元运算符的前/后要不要加空格
    "spaced-comment": 0,//注释风格要不要有空格什么的
    "strict": 2,//使用严格模式
    "use-isnan": 2,//禁止比较时使用NaN，只能用isNaN()
    "valid-jsdoc": 0,//jsdoc规则
    "valid-typeof": 2,//必须使用合法的typeof的值
    "vars-on-top": 2,//var必须放在作用域顶部
    "wrap-iife": [2, "inside"],//立即执行函数表达式的小括号风格
    "wrap-regex": 0,//正则表达式字面量用小括号包起来
    "yoda": [2, "never"]//禁止尤达条件

```