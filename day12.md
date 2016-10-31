# 面向对象
### jQuery基本结构
```
    (function ( global ) {
         var jQuery = function( selector, context ) {
             return new jQuery.fn.init( selector, context );
         };
         jQuery.fn = jQuery.prototype = {
             constuctor: jQuery
         };
         jQuery.extend = jQuery.fn.extend = function () {

         };
         var init = jQuery.fn.init = function( selector, context, root ) {
         };
         init.prototype = jQuery.fn;
         window.jQuery = window.$ = jQuery;
    }( window ));
```
### 为什么最外面创建一个匿名的构造函数？
>通过这个匿名的构造函数创建一个特殊的函数作用域，该作用域内的代码不会和已有的同名函数，方法，变量以及第三方库冲突。由于jQuery会在成千上万的JavaScript程序中应用，所以必须确保jQuery的代码不会受到其他任何代码的干扰，并且不能破坏和污染全局变量以至于影响到其他代码。这一点也是任何JavaScript库和框架所必备的功能。匿名函数最后手动把变量jQuery添加到window对象上，明确地使jQuery成为公开的全局变量，而其他部分都是私有的。(自调匿名函数后面的分号最好不要省略)

### 为什么自调函数设置参数window并传入window对象？
为什么自调函数设置参数window并传入window对象？
>通过传入window对象，可以使window对象变为局部变量，即把函数作为局部变量使用，这样在jQuery代码块中访问window对象时，不需要将作用于链退回到顶层作用域，从而可以更快地访问window对象；另外，将window对象作为参数传入可以在压缩代码时进行优化。

### 代码详解
```
(function ( window ) {
            // 厂函数就是对创建实例的过程进行一个封装，jQuery就是一个工厂函数，
            // 在jQuery内创建实例的构造函数就在jQuery的原型jQuery.prototype中，
            // （键）他的名为init，原型：jQuery.fn.init.prototype
            //后来把init独立出来成为一个单独的构造函数,var init = jQuery.fn.init，可以直接用了init.prototype
            // 为了方便，把jQuery.prototype简写成jQuery.fn）

            //这是jQuery工厂函数
    var jQuery = function( selector, context ) {
        return new jQuery.fn.init( selector, context );
    };

            // 工厂函数的原型起个别名
    jQuery.fn = jQuery.prototype = {
        constuctor: jQuery
    };

            // 给JQuery工厂函数自身 以及 JQuery工厂函数的原型中添加extend方法，
            // 以便于对JQuery进行功能扩展。
    jQuery.extend = jQuery.fn.extend = function (obj) {
                //外暴露jQuery，通过jQuery.fn.extend可以向jQuery原型中添加方法
                //传入一个对象，里面有许多方法，遍历对象可把方法一个一个加上去
        for ( var key in obj ) {
             this[key] = obj[key];//this指的是jQuery.fn
        }
    };

            // init是JQuery中真正的构造函数，最终所有代码都会走这里
    var init = jQuery.fn.init = function( selector, context, root ) {

    };

            //jQuery有许多插件，这些插件其实都是往jQuery的原型中添加方法，因为我们对外暴露的是jQuery，
            //所以第三方插件想要扩充jQuery就得往jQuery或者其原型中加
            //但是加入到jQuery.fn中的方法init创建出来的实例无法使用
            //为了让init构造函数创建出来的对象可以使用第三方扩充进来的方法
            //就把构造函数的原型与工厂函数的原型保持一致
    init.prototype = jQuery.fn;

            //今后给$的原型fn添加属性和方法就相当于添加到构造函数的原型中了，，
            // 当然通过jQuery的extend方法也可以，更方便，
            // jQuery中的extend方法可以给工厂函数jQuery或其原型添加方法，也可以添加第三方插件

            // 把工厂函数通过两个变量jQuery以及$暴露到全局。
    window.jQuery = window.$ = jQuery;

}( window ));

        //当我们用$("div")时其实返回的是构造函数init的一个实例，这个实例可以使用jQuery原型上的方法
        //调用jQuery()返回的对象实际上是构造函数jQuery.fn.init()的实例，由于设置了init.prototype = jQuery.fn，init的实例就可以访问了。
        //构造函数jQuery.fn.init()负责解析参数selector和context的类型，并执行相应代码，最后返回他的是jQuery.fn.init()的实例；
    $.fn.extend({
            alert: function (par) {
                alert(par);
                return this;
            },
             con: function (par) {
                console.log(par);
                return this;
            }
    });
               // 链式编程原理是return this
    $('div').alert(10).con(100);
```

### 插件机制

- jQuery.fn.extend(object)
    - 扩展 jQuery 元素集来提供新的方法（通常用来制作插件）。
- jQuery.extend(object)
    - 扩展jQuery对象本身。用来在jQuery命名空间上增加新函数。
