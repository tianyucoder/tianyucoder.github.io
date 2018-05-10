---
layout: post
title: "JS中addEventListener的事件捕获、事件冒泡"
date: 2018-05-10 
description: "JavaScript，前端，JS绑定事件"
tag: JS技术 
---   

## 概要：
&emsp;&emsp;在最近的项目中涉及到了一些复杂的DOM操作和事件绑定，而且还在addEventListener这个方法上踩了一个小坑，所以在这里仔细的总结一下这个方法，希望对各位小伙伴有帮助。
 

## 一、addEventListener基本形式及参数：

&emsp;&emsp;首先我们都了解：addEventListener()函数是最常用的用来为指定的dom元素动态绑定事件，并且这个函数有三个参数，具体如下：

	btn.addEventListener(type,listener,useCapture)
	
 
 **参数说明：**
> **type：**监听事件的类型，如：click、load... 等等。  
> **listener：**执行监听事件的函数名称。  
> **useCapture：** 是否采用捕获模式(布尔类型)，当值为**flase**时：不采用捕获模式（即：采用冒泡模式）；当值为**true**时：采用捕获模式；系统默认为false，即：采用冒泡模式，这个参数不给时，系统默认也会给一个fasle。不过为了最好的兼容性，最好还是指定具体的true、false，稍后我们就这个参数进行进一步探讨。

## 二、最常见的用法：
&emsp;&emsp;这里不多赘述，就是正常的加监听方法：<br>
HTML代码：

	<button id="button0"> </button>
JS代码：

	var btn = document.getElementById("button0");
    btn.addEventListener("click",btnHandler,false);
    function btnHandler(){
        console.log("hello")// 处理逻辑
    }
	
&emsp;&emsp;这样我们点击id为button0的元素时，控制台就会输出”hello”。	
## 二、冒泡模式
&emsp;&emsp;接下来我们就围绕第三个参数来探讨问题，为了更清晰的解释问题，我们制造一个层级嵌套的四个div，HTML代码如下：

	
	 <style>
        #div0{
            width: 200px;
            height: 200px;
            background-color: gray;
        }
        #div1{
            width: 160px;
            height: 160px;
            background-color: orange;
        }
        #div2{
            width: 100px;
            height: 100px;
            background-color: skyblue;
        }
        #div3{
            width: 60px;
            height: 60px;
            background-color: pink;
        }
    </style>
    </head>
    <body>
    
    <div id="div0">
    	<div id="div1">
        	<div id="div2">
            	<div id="div3">
                
            	</div>
        	</div>
    	</div>
    </div>
    
    </body>
 
 &emsp;&emsp;网页展示如下图：
 
  ![网页展示](http://oi5hiw2r7.bkt.clouddn.com/div.png?imageMogr2/thumbnail/300x400>/blur/1x0/quality/100)
  
 &emsp;&emsp;当前结构的3D侧视图如下：(由内到外分别为：div0、div1、div2、div3)
  
  ![3D展示](http://oi5hiw2r7.bkt.clouddn.com/div3D.png?imageMogr2/thumbnail/300x400>/blur/1x0/quality/100)
  
 &emsp;&emsp;接下来我们给每一个div都绑定监听方法，让点击当前div时，控制台输出当前div名称，所有的useCapture都设置为fasle，使用冒泡模式，JS代码如下：

	var div0 = document.getElementById("div0")
    var div1 = document.getElementById("div1")
    var div2 = document.getElementById("div2")
    var div3 = document.getElementById("div3")

    div0.addEventListener("click",div0Handler,fasle);
    div1.addEventListener("click",div1Handler,false);
    div2.addEventListener("click",div2Handler,false);
    div3.addEventListener("click",div3Handler,false);

    function div0Handler(){
        console.log("div0")
    }
    function div1Handler(){
        console.log("div1")
    }
    function div2Handler(){
        console.log("div2")
    }
    function div3Handler(){
        console.log("div3")
    }
	
&emsp;&emsp;下面我们开始观察结果：

> **1.**点击div0（灰色的），控制台先后输出为：```div0```。   
> **2.**点击div1（橙色的），控制台先后输出为：```div1```、```div0```。   
> **3.**点击div2（蓝色的），控制台先后输出为：```div2```、```div1```、```div0```。   
> **4.**点击div4（粉色的），控制台先后输出为：```div3```、```div2```、```div1```、```div0```。   

&emsp;&emsp;**上述输出结果的说明：**<br>
&emsp;&emsp;输出1不难理解，点击div0即输出当前的“div0”，那么输出2怎么解释呢？我们可以这样去理解输出2，当点击div0时，会正常输出“div0”，但是输出之后事情并没有结束，因为此时方法的useCapture为fale，即当前为冒泡模式，所以点击div1之后，事件会向它的父元素(div1)扩散，由于父元素也绑定了监听事件，所以父元素对应的监听函数也执行了，所以输出了“div1”，所以也就不难理解输出3、输出4的结果了。<br>
**冒泡模式总结：**<br>
&emsp;&emsp;冒泡模式的“冒泡”，可以理解为：从当前元素触发监听事件后，向自己的外层元素一层一层扩散（冒泡），一层一层的父元素都会响应自己的监听函数。

  
 
 
 
 



