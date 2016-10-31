# 面向对象第十三天
- 封装jQuery过程中会用到很多方法工具，我们把这些方法工具都放在jQuery身上，作为jQuery的静态方法，当我们在jQuery这个库中对使用者传入的参数进行处理或封装相应的方法时需要到这些工具可以直接调用我们事先封装好的放在jQuery身上公共工具。
- querySlector:返回的是一个DOM元素，如果没有获取到，返回null
-  querySlectorAll:返回的是一组DOM元素(伪数组对象)，如果没有获取到，返回一个空的伪数组对象。
-  charAt();获取字符串指定位置的字符
```
        var str = 'abc';
        console.log(str[2]);//c
        console.log(str.charAt(0));//a
        console.log(str.charAt(2));//c
```
- window的length属性
    - 每个iframe里面，都有属于自己的window
    - 每个window对象里面都有length属性，代表当前window中iframe的数量
```
      <iframe></iframe>
<iframe></iframe>
    <script>
        console.log(window.length);//2
    </script>
```

- var e=jQuery()，实例e通过init在外界能e.init()找到构造函数，通过e.constructor能找到工厂函数；
- 通过jQuery()/$()在里面传入参数可以获取到一个init的空实例，相当于`new init()`并返回，要通过init构造函数给实例添加内容，init构造函数里面的this指向空实例，init里面实现的就是让他的所有实例都变成一个伪数组，一个包含与传入参数相关的所有对象的及许多方法的伪数组，这个$()伪数组还可以调用里面的方法，因为init里面的方法就是给$()这些实例使用的。对于传入$()中的参数要进行判断并用不同的方法对传入的参数进行处理。我们在jQuery的身上添加了许多静态工具方法，这些静态方法可用于绘制init构造函数，比如判断数据类型，是函数、字符串、数组还是dom对象，或者处理数据返回相应的值等等，只是我们把这些工具方法单独提炼出来放在了jQuery身上，成为公共的方法，在jQuery内部和外部都可以使用。
    - 如果传入的是空或者空字符窜或者0:返回实例自己（空实例）；
    - 传入函数：jQuery要在内部对传入的函数进行调用，调用传入函数的过程也被封装成了jQuery的静态方法ready；
    - 传入字符串：进一步判断是否为HTML判断，是的话则调用jQuery静态方法parseHTML解析该字符串返回一个伪数组，然后将返回结果push到实例中，不是的话就看做是选择器用querySelectorAll获取伪数组对象平铺到实例中；
    - 传入DOM对象：直接push到实例中；
    - 传入数组：是数组就平铺；
    - 其他所有直接push；
- jQuery中的append与appendTo； 
```
 console.log($(window)); //{0:window,length:1}直接把window自己添加到了实例中
        console.log($(1)); //{0:1,length:1} 把1添加到实例中
        console.log($(true)); // {0:true,length:1}把true添加到实例中
        console.log($('abc'));  // {length:0},当做选择器去找，没找到，返回一个length为0的实例对象
        console.log($()); // 返回一个空实例对象
        console.log($("")); // 返回一个空实例对象
        console.log($('span')); // 返回实例对象，length为3，以下标的方式存储了所有的span
        console.log($('div')); // 返回实例对象，length为1，以下标的方式存储了所有的div

        // 如果给$传入的是html片段，$会自动把他们转换成DOM对象，然后添加到实例中
        console.log($('<div><a>1</a><a>2</a></div><div><a>11</a><a>22</a></div>').appendTo('body'));
        console.log($('<span>我是span1</span><span>我是span2</span>'));

        // 如果给$传入的是DOM元素，那么把这个DOM元素添加到实例中
        console.log($(document.getElementById('div')));

        // 如果给$传入的是数组，那么把数组中每一项值添加到实例中
        var arr = [1,2,3,4];
        console.log($(arr));

        // 如果给$传入的是伪数组，那么把数组中每一项值添加到实例中
        var spans = document.getElementsByTagName('span');
        console.log($(spans));

        // 如果给$传入的是伪数组，那么把数组中每一项值添加到实例中
        var obj = { 0:111, 1:222, length:2 };
        console.log($(obj));
```

### 创建jQuery库
```
    (function(w){
    		//工厂函数jQuery new init实例
    		function jQuery(selector){
    			return new init(selector);
    		}
    		jQuery.fn=jQuery.prototype={
    			constructor:jQuery,
    			isReady:false
    		};
    		//给jQuery添加静态方法extend，此方法可以给jQuery及其原型添加方法
    		jQuery.extend=jQuery.fn.extend=function(obj){
    			for(var key in obj){
    				this[key]=obj[key];
    			}
    		};
    		//通过jQuery的extend方法给jQuery添加方法
    		jQuery.extend({
    			isFunction:function(fn){//typeof判断函数
    				return typeof fn ==="function";
    			},
    			isString:function(str){//typeof判断字符串
    				return typeof str ==="string";
    			},
    			isHTML:function(html){//判断HTML片段
    				return html[0]==="<"&&html[html.length-1]===">"&&html.length>=3;
    			},
    			isDOM:function(dom){//判断dom对象
    				return !!dom&&!!dom.nodeType;
    			},
    			isWindow:function(win){//判断window对象
    				return !!win&&win.window===win;
    			},
    			isArrayLike:function(likeArray){//通过length属性判断数组及伪数组
    				if(jQuery.isFunction(likeArray)||jQuery.isWindow(likeArray)){
    					return false;//函数及window也有length属性，要刨除
    				}else{
    					return !!likeArray&&typeof likeArray==="object"&&"length" in likeArray&&(likeArray.length===0||[likeArray.length-1] in likeArray);
    					//判断是否为object类型是为了刨除基本数据类型数字，如果传入数字到这里用in运算符会报出语法错误。
    				}
    			},
    			parseHTML:function(html){//解析HTML片段
    				var tmpDiv=document.createElement("div");
    				tmpDiv.innerHTML=html;
    				return tmpDiv.children;
    			},
    			ready:function(fn){//调用传入的函数
    				if(jQuery.fn.isReady){
    					return fn();
    				}else{
    					if(document.addEventListener){
    						document.addEventListener("DOMContentLoaded",function(){
    							jQuery.fn.isReady=true;
    							fn();
    						});
    					}else{
    						document.attachEvent("onreadystatechange",function(){
    							jQuery.fn.isReady=true;
    							fn();
    						});
    					}
    				}
    			}
    			//多次用$()传入函数多次执行函数调用，需要判断dom结构是否加载完毕才能调用函数，
    			//判断dom树是否加载完毕的事件可同DOMContentLoaded，这个事件触发比onload要快，
    			//IE9和现代浏览器都支持该事件。但是ie8不支持，他支持onreadystatechange，这个事件也比onload要快。

    		});
    		var init=jQuery.prototype.init=function(selector){
    			if(!selector){//传入空与空串与0，返回空自身
    				return this;
    			}else if(jQuery.isFunction(selector)){//传入函数就调用，ready方法可实现传入函数的调用
    				jQuery.ready(selector);
    			}else if(jQuery.isString(selector)){//传入字符串
    				if(jQuery.isHTML(selector)){//传入HTML片段字符串
    					 [].push.apply(this,jQuery.parseHTML(selector));
    				}else{
    			            try {
                                         [].push.apply( this,document.querySelectorAll( selector ) );
                          //如果这里面有错误就会用catch来捕获错误，并作出相应的处理，
                        //然后后面的代码还会继续执行，如果没有错误就正常执行语句。
                                     }catch(e){}
    				}
    			}else if(jQuery.isDOM(selector)){
    				 [].push.call(this,selector);
    			}else if(jQuery.isArrayLike(selector)){
    				 //[].push.apply(this,selector);,解决ie8不能识别传入自定义伪数组
    				  [].push.apply( this, slice.call( selector ) );
    			}else{
    				 [].push.call(this,selector);
    			}
    		}
    		init.prototype=jQuery.prototype;
    		$(function(){});//解决延时定时器里面的函数无法调用的问题
    		w.jQuery=w.$=jQuery;	
    }(window));
    
    setTimeout(function () {
			$(function () {
				console.log('传入$的函数');
			});
			$(function () {
				console.log('过一会传入$的函数');
			});
	}, 3000);，
 	//这时这个定时器里面的函数不会被调用，因为DOMContentLoaded事件是在dom树加载完成后就触发
	//，三秒后dom早已加载完毕，这时再$(function(){})传入函数，DOMContentLoaded事件根本不会被触发
 	//，里面的调用方法就不会执行，阀门isReady永远都变不了true，后面的函数更不能被调用。
 	//因此在延时定时器里面的函数永远都无法执行。
 	//要解决这个问题，要在DOMContentLoaded事件触发时就执行以下函数调用
 	//，即在dom结构中直接$(function(){})传入一次函数，dom树加载完毕立即触发DOMContentLoaded事件，
 	//让里面的isReady变为true，而后传入的函数就可以正常执行了。
```
> 给jQuery添加静态方法：isFunction,isString，isDOM，isHTML，isWindow，isLikeArray，parseHTML（解析HTML字符串返回），ready，init构造函数对new出来的实例传参进行判断处理返回伪数组实例对象。

