---
layout: post
title: "CommonJS_Node(服务端)模块化教程"
date: 2018-06-02
description: "JavaScript，前端"
tag: JS模块化 
--- 

### **1.Node.js模块化教程**
**前言：**

> 1. CommonJS服务端模块化的具体代表就为Node.js，在Node中，不需要编码人员手动通过CommonJS将各个模块进行合并，只需要在入口JS启动服务即可，模块关联调用等，均有Node内置的CommonJS引擎完成。<br>
> 2. 暴露模块的核心，是暴露exports对象，通过```module.exports```这种方式暴露，会对当前js之前的暴露产生覆盖，采用```exports.暴露对象```方式不会产生覆盖。

**1. 安装node环境（具体步骤略）**	<br>
**2. 创建项目结构：**	<br>

	  |-modules
	    |-module1.js
	    |-module2.js
	    |-module3.js
	  |-app.js
	  |-package.json
	    {
	      "name": "commonJS-node",
	      "version": "1.0.0"
	    }

**3. 下载第三方模块（如果项目需要）**<br>
&emsp;这里以npm的uniq模块为例：```npm install uniq --save```<br>

**4. 模块化编码**<br>
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
    
&emsp; app.js 内容：
    
    /*
    
      1. 定义暴露模块:
        module.exports = value;
        exports.xxx = value;
        
      2. 引入模块:
        var module = require(模块名或模块路径);
        
     */
    "use strict";//启用严格模式
    //引用自定义模块
    let module1 = require('./modules/module1')
    let module2 = require('./modules/module2')
    let module3 = require('./modules/module3')
    //引用第三方模块
    let uniq = require('uniq')
    let fs = require('fs')
    
    //使用模块
    module1.foo()
    module2()
    module3.foo()
    module3.bar()
    
    console.log(uniq([1, 3, 1, 4, 3]))
        
**5. 通过node运行app.js**
  
> 1. 命令: ```node app.js```	
> 2. 工具: 右键-->运行
