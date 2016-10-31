[TOC]
# 面向对象第十七天
## JS知识点
### 1.关于typeof
- typeof一般用来判断基本数据类型。是一个操作符而不是函数，圆括号可有可无。
- typeof 返回值有：string，number，boolean，undefined，object ，function，
- 基本数据类型： Boolean、Number、String、Undefined、Null。这5种基本数据类型是按值访问的，因为可以操作保存在变量中的实际的值。
    - 基本数据类型中数字，字符串，布尔返回其对类型;
    - undefined返回undefined;
    - 九大内置构造函数及其他所有函数返回function；
    - 其他所有复杂类型对象和null返回object
 
### 2.数据类型转换 

#### 2.1转换成字符串类型
- 强制类型转换
    1. toString()方法：
        - 几乎每个值都有的toString()方法，但null 和undefined 值没有这个方法。
        - toString()可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。`10.toString(2); --->"1010"`
    2. String()，这个全局函数能够将任何类型的值转换为字符串。
- 隐式类型转换
    1. 拼接字符串： 
        - 无论任何值只要加上空串`""`就可以转换成字符串：`console.log(6+"")`

#### 2.2转换成数值类型
- 强制类型转换
    1. Number()能够将任何类型的值转换为数值。如果要转换成字符串的数值有一个不是数值的字符返回NaN.
        - var num1 = Number("Hello world!"); //NaN
        - var num2 = Number(""); //0
        - var num3 = Number("000011"); //11
        - var num4 = Number(true); //1
        - var num5 = Number(null); //0
    2. parseInt()函数转换字符串为数值：
        - parseInt("  123")----->123
        - parseInt("123px")----->123
        - parseInt("123.123")----->123
        - parseInt("")----->NaN
        - parseInt("abc123")----->NaN
    3. parseFloat()函数函数转换字符串为数值
        - parseFloat("123.123.23.45")----->123.123 //保留第一个小数点
- 隐式类型转换
    1. 减乘除具有算数功能可将非数字类型的字符串转换成数字
        - console.log("123"-0)---->123
        - console.log("123"/1)---->123
        - console.log("123px"-0)---->NaN

#### 2.3转换成布尔类型
- 强制类型转换
    1. Boolean(),所有的值都可以转换成布尔类型.
- 隐式类型转换
    1. 用!!将一个值转换成布尔类型;

### 3.split() 、join() 的区别
- split():将字符串转换成数组；
```
    var str1 = "1,2,3";
    var str2 = "1|2|3";
    var arr1 = str1.split(",");    //  arr1 = [1,2,3]
    var arr2 = str2.split("|");    //  arr2 = [1,2,3]

```
- join():将数组转换成字符串；
```
    var arr=["a","sd","as"];
    var str=arr.join("")    // "asdas"
```

### 4.数组方法pop(),push(),unshift(),shift()
- push()//从后面推入元素，可推入多个，返回新数组的length，原数组被修改；
- unshift() //从前面添加元素，返回新数组的length，原数组被修改；
- pop()  //从后面删除元素(一个)，返回被删除的元素，原数组被修改；
- shift() //从前面删除元素(一个)，返回被删除的元素，原数组被修改。

### 5.事件绑定和普通事件有什么区别
- 普通添加事件的方法不支持添加多个事件，最下面的事件会覆盖上面的，而事件绑定（addEventListener）方式添加事件可以添加多个。
- addEventListener支持事件冒泡+事件捕获，不兼容低版本IE
- 普通事件无法取消

### 6.事件流
>如果我们给一个按钮注册了点击事件，当点击按钮时，单击事件不仅仅发生在按钮上，在单击按钮的同时，也单击了按钮的父元素，父元素的父元素，甚至也单击了整个页面。
>事件流描述的是元素从页面中接收事件的顺序。两家浏览器公司提出的两种事件流：IE 的事件流是事件冒泡流，而Netscape Communicator 的事件流是事件捕获流。

#### 6.1. IE事件流----->事件冒泡
> 事件开始时由具体的元素接收，然后沿着DOM树逐级向上传播，在每一级节点上都会发生，直到传播到document对象。
> 不是所有的事件都能冒泡。以下事件不冒泡：blur、focus、load、unload（关闭页面）
> 兼容性:所有现代浏览器都支持事件冒泡,只是具体实现上有些差异。
```
    <!DOCTYPE html>
    <html>
        <head>
            <title>Event Bubbling Example</title>
        </head>
        <body>
            <div>
                <input type="button" id="btn" value="点击我">
            </div>
        </body>
    </html>
    //单击页面中的按钮，click 事件会按照如下顺序传播：(1) <input>;(2) <div>;(3) <body>;(4) <html>;(5) document
```

#### 6.2. Netscape事件流----->事件捕获
- 事件从document 对象开始传播，document对象首先接收到click事件，然后事件沿DOM树依次向下，一直传播到事件的实际目标<input>.
- 上述案例：单击页面中的按钮，click 事件会按照如下顺序传播：(1) document;(2)  <html>;(3) <body>;(4)<div>;(5) <input>
- 兼容性：目前IE9、Safari、Chrome、Opera和Firefox都支持，老版本的浏览器不支持。

#### 6.3. DOM事件流:事件捕获阶段---->处于目标阶段---->事件冒泡阶段
- 兼容性：IE9、Opera、Firefox、Chrome、Safari都支持DOM事件流；IE8及更早版本不支持DOM事件流。

### 7.事件处理程序

#### 7.1. DOM0 级事件处理程序
```
    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
    alert("Clicked");
    };
```
>使用DOM0 级方法指定的事件处理程序被认为是元素的方法。程序中的this 引用当前元素
```
    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
    alert(this.id); //"myBtn"
    };
```

####　7.2. DOM2 级事件处理程序
> 绑定事件：addEventListener()；删除事件：removeEventListener()。
> 3个参数：
    - 要处理的事件名
    - 作为事件处理程序的函数
    - 一个布尔值
        - true，表示在捕获阶段调用事件处理程序；
        - false，表示在冒泡阶段调用事件处理程序。
> 好处:可以添加多个事件处理程序
> 事件处理程序在其依附的元素的作用域中运行,this指向当前
```
    var btn = document.getElementById("myBtn");
    btn.addEventListener("click", function(){
    alert(this.id);
    }, false)
```
>通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；移除时传入的参数与添加处理程序时使用的参数相同。这也意味着通过addEventListener()添加的匿名函数将无法移除.（上述函数无法移除，要移除得向里面有名的函数）

#### 7.3. IE事件处理程序
- 绑定事件：attachEvent()；删除事件：detachEvent()。
- 两个参数：
    - 事件处理程序名称
    - 事件处理程序函数
```
    var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick", function(){
    alert("Clicked");
    });
```
> 在IE中使用attachEvent()用DOM0级方法的事件处理程序作用域作用域不同。
> DOM0级中，事件处理程序会在其所属元素的作用域内运行,this指向当前元素；
> 使用attachEvent()，事件处理程序会在全局作用域中运行，因此this等于window。
```
    var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick", function(){
    alert(this === window); //true
    });
```

#### 7.4. 阻止事件冒泡
- DOM标准事件流浏览器：
```
    var btn = document.getElementById("myBtn");
        btn.onclick = function(event){
        alert("Clicked");
        event.stopPropagation();//Function类型，取消事件的进一步捕获或冒泡
    };
```
- Ie8及其更早：
```
    var btn = document.getElementById("myBtn");
        btn.onclick = function(event){
        alert("Clicked");
        window.event.cancelBubble = true;
        // 布尔类型，表明是否可以取消事件的默认行为
    };
```
- 封装阻止事件冒泡函数：
```
      function stopPropagation(event)｛
        event= event || window.event; 
        if(event. stopPropagation){
            event. stopPropagation();
        }else{
            event. cancelBubble = true;
      }
    }
```

#### 7.5.阻止事件传递后的默认处理
- DOM标准通过调用event对象的preventDefault()方法即可。
```
    function someHandle(){ 
        window.event.returnValue= false;
   }
```
- 在IE下,通过设置event对象的returnValue为false即可。
```
    function someHandle(event){ 
        event.preventDefault();
    }
```
- 兼容性封装
```
    function someHandle(event){ 
        event= event || window.event; 
        if(event.preventDefault){
            event.preventDefault();
        }else {
            event.returnValue = false;
        }
    }
```

### 8.call和apply的区别

## 9.添加 删除 替换 插入到某个接点的方法
- obj.appendChid(要添加的节点)
- obj.insertBefore(要插入的节点，作为参照的节点)
- obj.replaceChild(要插入的节点，被替换的节点)
- obj.removeChild(要移除的节点)

### 10.javascript的本地对象，内置对象和宿主对象
- 1.本地对象：ECMA-262定义为独立于宿主环境的ECMAScript实现的对象。
本地对象：Object、Function、Array、String、Number、Date、RegExp、Boolean、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError、URIError ，就是ECMA-262定义的原生引用类型，没有实例化。
- 2．内置对象：ECMA-262 定义：由ECMAScript 实现提供的、不依赖于宿主环境的对象，这些对象在ECMAScript 程序执行之前就已经存在了。是已经实例化的对象。ECMA-262定义的内置对象只有两个：Global和Math。（根据定义他们也是本地对象）.
    + Global（全局）对象可以说是ECMAScript 中最特别的一个对象了，因为不管你从什么角度上看，这个对象都是不存在的，不能直接访问Global 对象。ECMAScript 中的Global 对象在某种意义上是作为一个终极的“兜底儿对象”来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。所有在全局作用域中定义的属性和函数，都是Global 对象的属性。所有原生引用类型的构造函数，像Object，Function，Date，Boolean，String，Number，RepExp，Array，Error这些内置构造函数也都是Global 对象的属性。
    + ECMAScript 虽然没有指出如何直接访问Global 对象，但Web 浏览器都是将这个全局对象作为window 对象的一部分加以实现的。因此，在全局作用域中声明的所有变量和函数，就都成为了window对象的属性。
- 3.宿主对象：由ECMAScript实现的宿主环境提供的对象。
所有非本地对象都是宿主对象。ECMAScript中的“宿主”就是我们网页的运行环境，即“操作系统”和“浏览器”。浏览器提供的对象（BOM）和DOM对象都是宿主对象。以及ECMAScript官方未定义的对象（通过ECMAScript程序创建的对象）都属于宿主对象。



