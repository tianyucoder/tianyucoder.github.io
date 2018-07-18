---
layout: post
title: "AMD-Require.JS 模块化教程"
date: 2018-06-03
description: "JavaScript，前端"
tag: JS模块化 
--- 

### **AMD-RequireJS模块化教程**
**前言：**

> 1. AMD-RequireJS只有浏览器端，没有服务器端。
> 2. AMD-RequireJS并不需要合并各个模块为一个最终js，靠在主入口配置requirejs.config实现实时加载模块。

**1. 下载require.js, 并引入：**

> 1. 官网下载地址: http://www.requirejs.cn/  
> 2. github下载地址: https://github.com/requirejs/requirejs 
> 3. 将require.js导入项目: js/libs/require.js
 
**2. 创建项目结构：**

	  |-js
	    |-libs
	      |-require.js
	    |-modules
	      |-alerter.js
	      |-dataService.js
	    |-main.js
	  |-index.html

**3. 定义require.js的模块代码：**

&emsp; dataService.js

	    define(function () {
	      let msg = 'atguigu.com'
	    
	      function getMsg() {
	        return msg.toUpperCase()
	      }
	    
	      return {getMsg}
	    })

&emsp; alerter.js

    define(['dataService', 'jquery'], function (dataService, $) {
      let name = 'Tom2'
    
      function showMsg() {
        $('body').css('background', 'gray')
        alert(dataService.getMsg() + ', ' + name)
      }
    
      return {showMsg}
    })

**4. 应用主(入口)js: main.js**
 
	  (function () {
	    //配置
	    requirejs.config({
	      //基本路径
	      baseUrl: "js/",
	      //模块标识名与模块路径映射
	      paths: {
	        "alerter": "modules/alerter",
	        "dataService": "modules/dataService",
	      }
	    })
	    
	    //引入使用模块
	    requirejs( ['alerter'], function(alerter) {
	      alerter.showMsg()
	    })
	  })()
  
        
**5. 页面使用模块:**

&emsp; ```<script data-main="js/main" src="js/libs/require.js"></script>```

> 备注：这里需要注意的是，src内引用的为require.js，我们的入口js配置在data-main属性中，这里容易写错。

<br>
**6. 使用第三方基于require.js的框架(例如jquery)**

&emsp;1.将jquery的库文件导入到项目: js/libs/jquery-1.10.1.js

&emsp;2.在main.js中配置jquery路径：

    
    paths: {
              'jquery': 'libs/jquery-1.10.1'
          }
    
&emsp;3.在alerter.js中使用jquery
    
    define(['dataService', 'jquery'], function (dataService, $) {
        var name = 'xfzhang'
        function showMsg() {
            $('body').css({background : 'red'})
            alert(name + ' '+dataService.getMsg())
        }
        return {showMsg}
    })
    


**7. 使用第三方不基于require.js的框架(angular)**

&emsp;1.将angular.js导入项目：js/libs/angular.js

&emsp;2.在main.js中配置:
    
    (function () {
      require.config({
        //基本路径，这里注意一旦配置虚拟路径后，相对的是项目文件夹根目录为起始位置。
        baseUrl: "js/",
        //模块标识名与模块路径映射
        paths: {
          //第三方库
          'jquery' : './libs/jquery-1.10.1',
          'angular' : './libs/angular',
          //自定义模块
          "alerter": "./modules/alerter",
          "dataService": "./modules/dataService"
        },
        /*
         配置不兼容AMD的模块
         exports : 指定与相对应的模块名对应的模块对象
         */
        shim: {
          'angular' : {
            exports : 'angular'
          }
        }
      })
      //引入使用模块
      require( ['alerter', 'angular'], function(alerter, angular) {
        alerter.showMsg()
        console.log(angular);
      })
    })()
 