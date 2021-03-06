#面向对象第10天
### 求圆上一点的坐标
- 在圆的45度地方，画一个点
- 求该点的x轴坐标：圆心x坐标 + 半径 * **Math.cos( angleToRadian(45) )**;
- 求该点的y轴坐标：圆心y坐标 + 半径 * **Math.sin( angleToRadian(45) )**;

### 文字预测
- 语法：**ctx.measureText( 文本 )**；
- 返回值：一个对象,该对象有一个width属性，代表这段绘制时所需的长度；

### 状态
- 状态就是变量或者属性不同时期的值。
- 状态的保存： 
> 就是把绘图对象，所有属性的状态值copy保留一份。
> **ctx.save();**

- 状态的回滚：
> 把之前保存的状态拿出来，覆盖当前的状态。
> **ctx.restore();**
- 状态的保存与回滚，与路径没有任何瓜葛。
- save方法会把绘图环境自身所有的属性保存一份，其中有些属性对我们来说是可见的，即可以通过ctx来获取到的属性值。

### 平移
- 语法：**ctx.translate( 在当前基础上x轴平移多少，在当前基础上y轴平移多少 );**
- 这里平移的是画布的坐标系，平移不会影响已经绘制好的图形，**平移会累加。**

### 旋转
- 语法：** ctx.rotate( 在当前基础上旋转多少弧度 )；**
- 这里旋转的是画布的坐标系，旋转不会影响已经绘制好的图形，**旋转会累加**。
- *默认情况下，旋转是依据坐标系的0,0选择的。*

### 缩放
- 语法：**ctx.scale( 在当前基础上x轴缩放多少倍，在当前基础上y轴缩放多少倍 )；**
- 这里缩放的是画布的坐标系，缩放不会影响已经绘制好的图形，**缩放会累加。**

### requestAnimationFrame
- 请求动画帧，这个方法的使用和setTimeout一致，只是不需要传时间。
- 优点：
    - 当浏览器要刷新页面的时候，才会调用传入该方法的回调，
    - 这个方法的执行频率会比较稳定，
    - 这个方法是专门用来开发动画使用的。

