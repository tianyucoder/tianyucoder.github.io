---
layout: post
title: "JavaScript中的DOM事件流"
date: 2018-05-10 
description: "JavaScript，前端，JS绑定事件"
tag: JavaScript 
---   

## 概要：
&emsp;&emsp;addEventListener()函数是最常用的用来为指定的dom元素动态绑定事件,在最近在addEventListener这个方法上踩了一个小坑，所以在这里仔细的总结一下这个方法，希望对各位小伙伴有帮助。


### **一、基本形式及参数：**

&emsp;&emsp;addEventListener()有三个参数，具体如下：

	addEventListener(type,listener,useCapture);
	

 **参数说明：**

> **type：**监听事件的类型，如：click、load... 等等。  
> **listener：**执行监听事件的函数名称。  
> **useCapture：** 是否采用捕获模式,**fasle**:冒泡模式；**true**:捕获模式。

<br>


### **二、比较常用的方式：**
&emsp;&emsp;例如给一个按钮增加监听方法，这样我们点击id为button0的元素时，控制台就会输出“hello”。	<br>

	<button id="button0">按钮0</button>

	var btn = document.getElementById("button0");
	
    btn.addEventListener("click",btnHandler);
    
    function btnHandler(){
        console.log("hello")// 处理逻辑
    }
	

### **三、冒泡模式**
&emsp;&emsp;接下来我们就围绕第三个参数来探讨问题，为了更清晰的解释问题，我们先写一个嵌套的四个div，HTML和CSS如下：

	
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
 
 &emsp;&emsp;浏览器展示效果如下图（为方便观察，我在每个div上加了文字说明）：

  ![网页展示](http://oi5hiw2r7.bkt.clouddn.com/div.png?imageMogr2/thumbnail/300x400>/blur/1x0/quality/100)


  
 &emsp;&emsp;当前结构的3D侧视图如下：(由内到外分别为：div0、div1、div2、div3)div0是根div元素。
  
  ![3D展示](http://oi5hiw2r7.bkt.clouddn.com/div3D.png?imageMogr2/thumbnail/300x400>/blur/1x0/quality/100)
  
 &emsp;&emsp;接下来我们给每一个div都绑定监听方法，让点击当前div时，控制台输出当前div名称，所有的useCapture都设置为fasle，使用冒泡模式，JS代码如下：

	 // 获取每个div的DOM对象。
	 var div0 = document.getElementById("div0")
    var div1 = document.getElementById("div1")
    var div2 = document.getElementById("div2")
    var div3 = document.getElementById("div3")
	 // 给每个div增加监听事件。
    div0.addEventListener("click",div0Handler,false);
    div1.addEventListener("click",div1Handler,false);
    div2.addEventListener("click",div2Handler,false);
    div3.addEventListener("click",div3Handler,false);
	 // div0的回调函数
    function div0Handler(e){
        e=e || window.event;
        console.log("div0")
    }
    // div1的回调函数
    function div1Handler(e){
        e=e || window.event;
        console.log("div1")
    }
    // div2的回调函数
    function div2Handler(e){
        e=e || window.event;
        console.log("div2")
    }
    // div3的回调函数
    function div3Handler(e){
        e=e || window.event;
        console.log("div3")
    }
	
&emsp;&emsp;下面我们开始观察结果：

> **输出1：**点击div0（灰色的），控制台先后输出为：```div0```。   
> **输出2：**点击div1（橙色的），控制台先后输出为：```div1```、```div0```。   
> **输出3：**点击div2（蓝色的），控制台先后输出为：```div2```、```div1```、```div0```。   
> **输出4：**点击div3（粉色的），控制台先后输出为：```div3```、```div2```、```div1```、```div0```。   

&emsp;&emsp;**解读上述输出结果：**<br>
&emsp;&emsp;
输出1不难理解，点击div0即输出当前的“div0”，那么输出2怎么解释呢？<br>我们可以这样去理解输出2：
当点击div0时，div0的监听的回调函数会正常输出“div0”，但是输出之后事情并没有结束，**因为此时方法的useCapture为fale，即当前为冒泡模式，所以点击div1之后，事件会向它的祖先元素(div1)扩散，由于祖先元素也绑定了监听事件，所以祖先素对应的监听函数也执行了**，所以输出了“div1”，这就是**冒泡模式**，理解了输出2，所以也就不难理解输出3、输出4了。 
 
>**冒泡模式总结：**<br>
&emsp;&emsp;冒泡模式的“冒泡”，可以理解为：从当前元素触发监听事件后，向自己的祖先元素一层一层扩散（冒泡），一层一层的祖先素都会响应自己的监听函数,直到DOM的根元素，即冒泡模式的方向是：从自己到祖先元素。

<br>


### **四、捕获模式**
还是刚才的代码，我们开启你捕获模式，把事件监听部分代码改为如下：
	
	// 给每个div增加监听事件，这次为捕获模式，全部为true。
    div0.addEventListener("click",div0Handler,true);
    div1.addEventListener("click",div1Handler,true);
    div2.addEventListener("click",div2Handler,true);
    div3.addEventListener("click",div3Handler,true);

&emsp;&emsp;接下来观察输出结果：
	
> **输出1：**点击div0（灰色的），控制台先后输出为：```div0```。   
> **输出2：**点击div1（橙色的），控制台先后输出为：```div0```、```div1```。   
> **输出3：**点击div2（蓝色的），控制台先后输出为：```div0```、```div1```、```div2```。   
> **输出4：**点击div3（粉色的），控制台先后输出为：```div0```、```div1```、```div2```、```div3```。  

&emsp;&emsp;**解读上述输出结果：**<br>
&emsp;&emsp;是的，正如你所见，事件捕获与事件冒泡完全相反，在捕获模式中，出发一个元素监听时，先找到这个元素的根祖先元素，触发这个祖先元素的事件，然后再逐级触发子元素的事件，直到当前元素为止，即捕获模式的方向是：**从祖先元素到自己**。

### **五、冒泡与捕获同时存在，执行顺序是怎样的？**
&emsp;&emsp;首先我们明确一点就是：**浏览器优先处理捕获事件**，当冒泡与捕获同时存在的时候，浏览器先把当前元素层级中的捕获事件拿出来，按照捕获的顺序进行响应，随后再按照冒泡的顺序进行冒泡事件响应，我们把刚才的事件绑定改为如下：

    div0.addEventListener("click",div0Handler,true);// div0为捕获模式
    div1.addEventListener("click",div1Handler,false);// div1为冒泡模式
    div2.addEventListener("click",div2Handler,true);// div2为捕获模式
    div3.addEventListener("click",div3Handler,false);// div3为冒泡模式
	
 
 输出结果如下：
 
> **输出1：**点击div0（灰色的），控制台先后输出为：```div0```。   
> **输出2：**点击div1（橙色的），控制台先后输出为：```div0```、```div1```。   
> **输出3：**点击div2（蓝色的），控制台先后输出为：```div0```、```div2```、```div1```。   
> **输出4：**点击div3（粉色的），控制台先后输出为：```div0```、```div2```、```div3```、```div1```。 

&emsp;&emsp;**解读上述输出结果：**<br>

>**输出1：**div0为捕获，祖先元素无监听事件，只输出div0。   
>**输出2：**div1是冒泡模式，它的祖先元素div0是捕获模式，优先处理捕获，输出div0，然后输出div1。   
>**输出3：**读取当前层级：div2是捕获，祖先div1是冒泡，祖先div0是捕获，优先处理捕获，捕获的方向是：胸祖先到自己，所以输出：div0、div2，最后处理冒泡，输出div1。  
>**输出4：**读取当前层级：div3是冒泡，祖先div2是捕获，祖先div1是冒泡，祖先div0是捕获，
先处理捕获，捕获是祖先到自己，输出了：div0、div2，随后处理冒泡，冒泡是从自己到祖先，所以先输出自己，即div3，扩散（冒泡）给div1，div1输出。  


### **六、如何去阻止冒泡和捕获的这种事件传播？**
我们用stopPropagation（）方法阻止事件传播，我们把之间的监听事件全部改为冒泡模式，并且在每一个回调函数中阻止事件传播，修改之前代码如下：

    div0.addEventListener("click",div0Handler,false);
    div1.addEventListener("click",div1Handler,false);
    div2.addEventListener("click",div2Handler,false);
    div3.addEventListener("click",div3Handler,false);

    function div0Handler(e){
        e.stopPropagation();
        console.log("div0")
    }
    function div1Handler(e){
        e.stopPropagation();
        console.log("div1")
    }
    function div2Handler(e){
        e.stopPropagation();
        console.log("div2")
    }
    function div3Handler(e){
        e.stopPropagation();
        console.log("div3")
    }
 输出结果如下：
 
> **输出1：**点击div0（灰色的），控制台先后输出为：```div0```。   
> **输出2：**点击div1（橙色的），控制台先后输出为：```div1```。  
> **输出3：**点击div2（蓝色的），控制台先后输出为：```div2```。    
> **输出4：**点击div3（粉色的），控制台先后输出为：```div3```。

&emsp;&emsp;**解读上述输出结果：**<br>
因为所有的都是冒泡模式，方向是从自己到祖先元素，输出自己后，紧接着阻止了事件传播，所以就达到了点击谁，就输出谁，没有任何传播。那么如果把上述的冒泡全部改为捕获呢？我们尝试一下，修改代码如下：

    div0.addEventListener("click",div0Handler,true);
    div1.addEventListener("click",div1Handler,true);
    div2.addEventListener("click",div2Handler,true);
    div3.addEventListener("click",div3Handler,true);

    function div0Handler(e){
        e.stopPropagation();
        console.log("div0")
    }
    function div1Handler(e){
        e.stopPropagation();
        console.log("div1")
    }
    function div2Handler(e){
        e.stopPropagation();
        console.log("div2")
    }
    function div3Handler(e){
        e.stopPropagation();
        console.log("div3")
    }
 输出结果如下：
 
> **输出1：**点击div0（灰色的），控制台先后输出为：```div0```。   
> **输出2：**点击div1（橙色的），控制台先后输出为：```div0```。  
> **输出3：**点击div2（蓝色的），控制台先后输出为：```div0```。    
> **输出4：**点击div3（粉色的），控制台先后输出为：```div0```。

&emsp;&emsp;**解读上述输出结果：**<br>
因为所有的都是捕获模式，方向是从祖元素先到自己，无论点击谁，都先读取层级关系，都会找到祖先元素div0，然后都走div0的回调函数，但是div0的回调函数中，阻止了事件的传播。在执行了div0的回调函数后，一切都停止了，所以点击每个div都会输出div0。

### **七、总结**

> **1.冒泡模式：**先读取DOM层级关系，方向：从自己到祖先元素，浏览器默认采用这种方式处理。  
> **2.捕获模式：**先读取DOM层级关系，方向：从祖先元素到自己。  
> **3.冒泡与捕获同时存在：**先读取DOM层级关系，优先捕获事件处理，随后处理冒泡事件。  
> **4.最后一个参数：**为了最好的兼容性，最后一个参数尽量不为空，最好写上true还是fasle