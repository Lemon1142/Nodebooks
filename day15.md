# 面向对象第十五天
### 内置DOM集合对象
```
        var as = document.getElementsByTagName('a');
        var asArr = [].slice.call( as );
        for ( var i = 0, len = asArr.length; i < len; i++ ) {
            // 删除每一个元素
            as[i].parentNode.removeChild( as[i] );
        }
        /*
        * 为什么这里要把as转换为数组，而我们在框架中实现的不需要转换：
        * 因为这里通过document.getElementsByTagName等是HTMLCollection类型的集合对象
        * ，他会动态变化,所以需要把它先转换为数组，数组不会动态变化。
        * 而由document.querySelector获取的对象是NodeList类型的集合对象，
        * 
        * 而框架中，我们操作的是this实例，这个实例是我们自己创造的，不会动态变化。
        * 
        * */
```
### jQuery原型上操作dom的方法
1. empty方法：清空实例中每个对象中的内容，并返回这个实例`$("div").empty()`，每个div中都空了，谁调用就把谁里面的东西都清空；
2. remove方法：清空实例中每一个对象中的内容，并把自己自身也移除掉`$("div").remove()`，div也都没了，谁调用就把谁及其内容全部清空；
3. appendTo方法：
    - `$('<span>1121</span>').appendTo('div')`往每一个div中都添加了这个`<span>1121</span>`标签;
    - `$('a').appendTo('div')`,往每一个div中添加指定的`$('a')`里面的所有`a`标签，原来位置上的那些`a`标签被添加上去不在了，不够的那些`a`都clone出来填上去;
    - 可以appendTo一个选择器,可以appendTo一个jQuerty对象`$('a').appendTo($('div'))`,可以appendTo一个DOM对象`$('a').appendTo(document.body)`，传什么格式都可以;
    - 返回值是所有添加进去的元素组成的新实例，所有a包括clone的，谁调用就把谁全部添加到每个容器中；
4. append方法：
    - append()可以传 DOM 或者 jQuery实例 或者 文本(没有选择器一说),传入字符串的话会被当做是文本添加进去，但是HTML格式的字符串除外；
    - `$('div').append($('a'))`,给每一个`div`添加所有`a`元素；
    - `$('div').append('a')`,给所有div添加a文本;
    - 返回值：append返回this自身；
5. prependTo方法：
    - ` $('a').prependTo('div')`，把所有`a`添加到`div`里面的最前面；
    - 传dom，选择器，jQuery实例都行；
    - 返回值是所有添加进去的元素组成的新实例，所有a包括clone的，谁调用就把谁全部添加到每个容器中；
6. prepend方法：
    -  prepend()可以传 DOM 或者 jQuery实例 或者 文本(没有选择器一说),传入字符串的话会被当做是文本添加进去，但是HTML格式的字符串除外；
    - `$('div').prepend($('a'))`,给每一个前面`div`添加所有`a`元素；
    - `$('div').prepend('a')`,给所有div前面添加a文本;
    - 返回值：append返回this自身；
```
    $.fn.extend({
            //清空实例中每个对象中的内容，并返回这个实例；
            empty:function(){
                this.each(function(){
                    this.innerHTML="";
                });
                return this;
            },
            //清空实例中每一个对象中的内容，并把自己自身也移除掉；
            remove:function(){
                this.each(function(){
                    this.parentNode.removeChild(this);//this只向遍历后的每一个值
                });
            },
            appendTo:function(selector){//把this中所有元素添加到$selector里面每一个对象中,添加到$selector第一个中的是其自身，剩下的是clone的
                var $selector=jQuery(selector);
                var result=[],temp; //this中所有元素及他的clone元素,即所有被添加元素，最终返回
                this.each(function(){
                    var self=this;
                    $selector.each(function(index,val){
                        temp=index==0?self:self.cloneNode(true);
                        this.appendChild(temp);
                        result.push(temp);
                    });
                });
                return jQuery(result);
            },
            append:function(contents){
                if(jQuery.isString(contents)){//如果是字符串，把他添加到this中每一个对象的内容的后面
                    this.each(function(){
                        this.innerHTML=this.innerHTML+contents;
                   })
                }else{
                   jQuery(contents).appendTo(this);
                }
                return this;
            }, 
            prependTo:function(selector){
                //把this添加到指定元素的前面，可以传dom，jQuery实例，选择器
                var $selector=jQuery(selector),temp,result=[],self;
                                /* for(var i=0;i<this.length;i++){
                                        for(var j=0;j<$selector.length;j++){
                                            if(j==0){
                                                temp=this[i];
                                            }else{
                                              temp=this[i].cloneNode(true);
                                            }
                                            $selector[j].insertBefore(temp,$selector[j].firstChild);
                                         result.push(temp);
                                     }
                                    }*/
                this.each(function(){
                    self=this;
                    $selector.each(function(index,val){
                        temp=index==0?self:self.cloneNode(true);
                        this.insertBefore(temp,this.firstChild);
                        result.push(temp);
                    });
                })
            },
            prepend:function(contents){
                //往this中添加contents
                if(jQuery.isString(contents)){//传入字符串当做文本传入
                    this.each(function(){
                        this.innerHTML=contents+this.innerHTML;
                    });
                }else{
                    $(contents).prependTo(this);
                }
                return this;
            }

        });
```
### jQuery原型上操作类名的方法
1. hasClass:只要有一个元素存在指定类名，那么就返回true，`$('div').hasClass('you')`;
2. removeClass:
    - ` $('div').removeClass()`,传空所有元素的所有类名都没有了；
    -  `$('div').removeClass("bb")`,将所有元素中有此类名的类名移除；
3. addClass:` $('div').addClass("bb")`;
4.toggleClass: `$('div').toggleClass('abc')`有就删除，没有就添加;
```
    jQuery.fn.extend({
            //判断有没有指定的类名，返回布尔值,$("div")中只要有一个div的类名中有就返回true
            //要算上空格，否则"a".indexOf("asff b vbf")也算是有a这个类名
            hasClass:function(name){//大于等于0，就代表有
                                /* for(var i=0;i<this.length;i++){
                                        if((" "+this[i].className+" ").indexOf(" "+name+" ")>-1){
                                            return true;
                                        }else{
                                            return false;
                                        }
                                    }*/
                var has=false;
                this.each(function(){
                    if((" "+this.className+" ").indexOf(" "+name+" ")>-1){
                        has=true;
                        return false;
                    }
                })
                return has;
            },
            removeClass:function(name){
                //如果传入空，移除所有类
                if(name===undefined){
                    this.each(function(){
                        this.className="";
                    });
                }else{
                    this.each(function(){
                        this.className=(" "+this.className+" ").replace(" "+name+" "," ").replace(/^\s*|\s*$/g,"");
                    });
                }
                return this;
            },
            addClass:function(name){
                this.each(function(){
                    if(!$(this).hasClass(name)){
                        this.className=(this.className+(" "+name+" ")).replace(/^\s*|\s*$/g,"");
                    }
                });
                return this;
            },
            toggleClass:function(name){
                this.each(function(){
                    if($(this).hasClass(name)){
                        $(this).removeClass(name);
                    }else{
                        $(this).addClass(name);
                    }
                });
            }

    });
```
