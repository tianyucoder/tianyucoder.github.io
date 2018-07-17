---
layout: post
title: "CommonJS-Browserify(浏览器端)模块化教程"
date: 2018-06-03
description: "JavaScript，前端"
tag: JS模块化 
--- 

### **CommonJS浏览器端模块化教程**
**前言：**

> 1. CommonJS在浏览器端的作用是：在项目投入生产环境之前，将分模块开发的js文件，进行整体打包，生成一个整体js，部署到生产环境（也很好理解，因为对于浏览器引擎，并不知道你的require是个什么东西）。<br>
> 2. 暴露模块的核心，是暴露exports对象，通过```module.exports```这种方式暴露，会对当前js之前的暴露产生覆盖，采用```exports.暴露对象```方式不会产生覆盖。

**1. 创建项目结构：**
  
	  |-js
	    |-dist //打包生成文件的目录
	    |-src //源码所在的目录
	      |-module1.js
	      |-module2.js
	      |-module3.js
	      |-app.js //应用主源文件
	  |-index.html
	  |-package.json
	    {
	      "name": "browserify-test",
	      "version": "1.0.0"
	    }
  
**2. 安装Browserify：**

&emsp; 说明：Browserify是目前最常用的CommonJS格式转换的工具。

&emsp; 全局安装: ```npm install browserify -g```（推荐）

&emsp; 局部安装: ```npm install browserify --save-dev```

&emsp; **这里推荐使用全局安装，这样可以在任何文件夹使用browserify命令**。

**3. 定义模块代码：**

&emsp;module1.js 内容：
    
    module.exports = {
      foo() {
        console.log('moudle1 foo()')
      }
    }
    
&emsp;module2.js 内容：
    
    module.exports = function () {
      console.log('module2()')
    }
    
&emsp;module3.js 内容：
    
    exports.foo = function () {
      console.log('module3 foo()')
    }
    
    exports.bar = function () {
      console.log('module3 bar()')
    }
    
&emsp;app.js (项目的主js)
    
    //引用自定义模块
    let module1 = require('./module1')
    let module2 = require('./module2')
    let module3 = require('./module3')
    
    let uniq = require('uniq')
    
    //使用npm第三方模块
    module1.foo()
    module2()
    module3.foo()
    module3.bar()
    
    console.log(uniq([1, 3, 1, 4, 3]))
    
**4. 打包处理js:**

* 第一步，合并模块： ```browserify js/src/app.js -o js/dist/bundle.js ```
* 第二步，页面引入：```<script type="text/javascript" src="js/dist/bundle.js"></script> ```