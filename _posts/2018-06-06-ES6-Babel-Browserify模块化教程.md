---
layout: post
title: "ES6-Babel-Browserify模块化教程"
date: 2018-06-05
description: "JavaScript，前端"
tag: JS模块化 
--- 
### **ES6-Babel-Browserify模块化教程**
**前言：**	

> 1. 应用最为广泛的一种模块化技术，建议各位熟练掌握。
> 2. ES6的模块化要借助：**Babel** 、**Browserify**来实现。
> 3. Babel主要用于将ES6语法转换为ES5，Browserify用于打包模块。
> 4. ES6的模块化一定记得写**.babelrc**配置文件。

**1. 定义工程package.json文件**		

&emsp;备注：package.json文件也可以通过```npm init```命令初始化。
  
	  {
	    "name" : "es6-babel-browserify",
	    "version" : "1.0.0"
	  }
  
**2. 安装：babel-cli 、babel-preset-es2015 、 browserify**

> 1. ```npm install babel-cli browserify -g ```
  2. ```npm install babel-preset-es2015 --save-dev ```	


**3. 配置项目.babelrc文件**	

```{ "presets": ["es2015"] }```
 
> 1. preset 预设是在告诉Babel它的任务(将es6转换成es5的所有插件打包)。
> 2. **注意：**如果忘记在项目中添加.babelrc文件，Babel会不知道自己要做什么，就会将项目的js复制一份，此时不会报错，js依然是ES6语法，直接去合并模块，会产生后续兼容性问题。


**4. 编码**

&emsp; js/src/module1.js（分别暴露）
    
    export function foo() {
      console.log('module1 foo()');
    }
    export function bar() {
      console.log('module1 bar()');
    }
    export const DATA_ARR = [1, 3, 5, 1]
    
&emsp; js/src/module2.js（统一暴露）
    
    let data = 'module2 data'
    
    function fun1() {
      console.log('module2 fun1() ' + data);
    }
    
    function fun2() {
      console.log('module2 fun2() ' + data);
    }
    
    export {fun1, fun2}
    
&emsp; js/src/module3.js （默认暴露）
    
    export default {
      name: 'Tom',
      setName: function (name) {
        this.name = name
      }
    }
    
&emsp; js/src/app.js
    
    //引入的自定义模块
    import {foo, bar} from './module1'
    import {DATA_ARR} from './module1'
    import {fun1, fun2} from './module2'
    import person from './module3'
    //引入的第三方模块，例如jquery
    import $ from 'jquery'
    
    $('body').css('background', 'red')
    
    foo()
    bar()
    console.log(DATA_ARR);
    fun1()
    fun2()
    
    person.setName('JACK')
    console.log(person.name);
    
> 备注：对于ES6的暴露而言，整个暴露没有之前Common.JS那样，本质是暴露module.export，也就是说都会被export这个对象包裹着，所以各个模块之间及容易产生明明冲突，所以在ES6中，每个模块暴露时都最好用一个对象去包裹，例如上文默认暴露的方式。

**5. 编译**

> 1. 使用Babel将ES6编译为ES5代码(但包含CommonJS语法) : ```babel js/src -d js/lib```
> 2. 使用Browserify编译js : ```browserify js/lib/app.js -o js/lib/bundle.js```

**6. 页面中引入测试**

	  <script type="text/javascript" src="js/lib/bundle.js"></script>
  
