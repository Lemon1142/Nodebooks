# 面向对象第十四天
#### 填充1
```
    (function(w){
            var version="1.1.1";
            var document=w.document;//提升变量查找效率

            var arr=[],//提高查找效率，简写代码，方便使用，在方法借用时可以直接写方法
            push=arr.push,
            slice=arr.slice;

            var obj={},
            toString=obj.toString,
            hasOwn=obj.hasOwnProperty;
    }(window))
```
#### 填充2：jQuery原型中的核心成员：
1. jquery属性，存储jQuery当前的版本；
2. constructor属性，存储jQuery工厂函数；
3. length属性，jQuery实例默认的length，防止$()时返回undefined；
1. toArray方法：转换为数组返回；`$('script').toArray()`
2.  get方法：获取指定下标的元素，返回dom对象；`$('span').get(0)`
    - 传入null，undefined和空全部获取并返回一个数组
    - 传入正数放回相应下标的dom对象
    - 传入负数，倒着数
1. slice方法：截取实例中部分元素，重新构成新的实例返回`$('span').slice(1, 2)`
2. eq方法：获取指定下标的元素，返回成新的实例；`$('span').eq(-1)`
    - 传入null，undefined和空会返回一个空实例
    - 传入正数放回相应下标的jQuery对象
    - 传入负数，倒着数
3. first方法，获取第一个元素，返回新的实例；`$('span').first()`
4. last方法，获取最后一个元素，返回新的实例；`$('span').last()`
5. push方法，给实例添加新元素；`$span.push( $('script').get(0) )`
6. sort方法，给实例进行排序；
    ```
        $span2.sort(function ( span1, span2 ) {
            return span1.innerHTML < span2.innerHTML;
        });
    console.log($span2); // 排序后的实例
    ```
7. splice方法，从指定下标删除指定数量的元素，或从指定下标替换指定数量的元素。`$('span').splice(1,3,'a','b')`
8. each方法，遍历实例所有的元素，把遍历到的数据分别传给回调,函数内部this指向遍历后的每一个value值，return false 可终止遍历；
    ```
        $.each( arr, function ( index, val ) {//注意参数顺序
            console.log( index, val );
        });
        
    $("div").each(function ( index, val ) {
            console.log( index, val );
        });
    ```
9. map方法，遍历实例所有的元素，把遍历到的数据分别传给回调，然后把回调的返回值组成一个数组返回，函数内部this指向window。
    ```
       $.map(obj, function (val, key) {
            console.log(val, key);
            return val * 10;
        });
        
    $("div").map(function (val, key) {
            console.log(val, key);
            return val * 10;
        });
    ```
10. 代码:
    ```
    // 原型简写&原型默认拥有的属性与方法
    jQuery.fn=jQuery.prototype={
                jquery:version,
                constructor:jQuery,
                isReady:false,//传入函数调用时的阀门
                length:0,
                toArray:function(){
                    return slice.call(this);
                },
                get:function(num){
                    if(num==null){
                        return this.toArray();
                    }else{
                        return num>0?this[num]:this[this.length-1];
                    }
                },
                eq:function(num){
                    var dom;
                    if(num==null){
                        return jQuery();
                    }else{
                        return  dom=this.get(num)?jQuery(dom):jQuery();
                    }
                },
                slice:function(){
                    return jQuery(slice.apply(this,arguments));
                },
                first:function(){
                    return eq(0);
                },
                last:function(){
                    return eq(-1);
                },
                each:function(fn){
                    return jQuery.each(this,fn);
                },
                //这个each的功能和jQuery上的each的功能是一样的，
                //所以只要在这这里执行jQuery的each函数就可以了，
                //函数的返回结果也是一样的，
                //所以只要返回jQuery的each函数的返回结果就可以了
                map:function(){
                    return jQuery.map(this,fn);
                },
                push: push,//把数组的push方法添加到原型中，供实例使用
                sort: arr.sort,// 把数组的sort方法添加到原型中，供实例使用
                splice: arr.splice
    };
```
#### 填充3
```
    // 添加静态方法
     jQuery.extend({
                each: function( obj, fn ) {
                    var i = 0, len, key;
                    if ( jQuery.isLikeArray( obj ) ) {
                        for ( len = obj.length; i < len; i++ ) {
                            if ( fn.call( obj[i], i, obj[i] ) === false ) {
                                break;
                            }
                        }
                    }else {
                        for ( key in obj ) {
                            if ( fn.call( obj[key], key, obj[key] ) === false ) {
                                break;
                            }
                        }
                    }

                    return obj;
                },

                map: function( obj, fn ) {
                    var result = [],
                        i = 0, len,
                        temp, key;

                    if ( jQuery.isLikeArray( obj ) ) {
                        for ( len = obj.length; i < len; i++ ) {
                            temp = fn( obj[i], i );
                            // 过滤null和undefined
                            if ( temp != null ) {
                                result.push( temp );
                            }
                        }
                    }else {
                        for ( key in obj ) {
                            temp = fn( obj[key], key );
                            if ( temp != null ) {
                                result.push( temp );
                            }
                        }
                    }
                    return result;
                }
     })
```
