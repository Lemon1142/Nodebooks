# 面向对象第九天
### forEach
- ES5提供的用来遍历数组的方法，该方法来自Array.
- 语法:数组.forEach(function())

### map
- ES5提供的用来遍历数组的方法，该方法来自Array.prototype.
- 会接收返回值，，把返回值组成一个新数组

### arc方法（画弧）
- 语法：ctx.arc(圆心x轴坐标，圆心y轴坐标，半径，起始弧度，结束弧度，是否逆时针画（可选，值：true）)。
    - 默认顺时针，值为false;
- arc方法会先使
- 180度 等于 1PI弧度
- 360度 等于 2PI弧度

### 绘制文字
1. 绘制描边文本
    - ctx.strokeText('文本',x,y,限制文本最大长度（可选）)
2. 绘制填充文本
    - ctx.fillText('文本',x,y,限制文本最大长度（可选）)
3. 文字测量
    - ctx.measureText(文本)；返回值：一个对象（里面有width属性，代表这段文字绘制时所需的长度）。
#### 文字水平排列方式
1. 设置文本样式
    - ctx.font = 语法和css一样
2. 设置文字水平排列方式
    - **ctx.textAlign=start(start)；**
    - 可选left,center,end,right;默认为start;

#### 文字垂直排列方式
1. 设置文字垂直排列方式
    - **ctx.textBaseline=top,middle,bottom,alphabetic,hanging,ideographic**
    - 默认值为 **alphabetic （字母基线）**
    - top在四线三格的上面
    - middle 在四线三格的中间
    - bottom在四线三格的下间；
    - alphabetic在四线三格中第三条线；
    - hanging相当于四线三格中第一条线；
    - ideographic表示和bottom基本一致。

### 绘制图像 
1. 绘制图像，drawImage三参数：
    - ctx.drawImage( 图像资源，x坐标，y坐标 )
    - 把图像绘制到指定的位置。
    - 在使用某个img之前，必须监听它onload事件
         ```
         img.onload = function () {
            ctx.drawImage( img, 10, 10 );
            }
         ```
2. 绘制图像，drawImage五参数：
    - ctx.drawImage( 图像资源，x坐标，y坐标, 图像显示的宽，图像显示的高 )
    - 把图像按照指定的大小绘制到指定的位置。
    - 在使用某个img之前，必须监听它onload事件
3. 绘制图像，drawImage九参数：
    - ctx.drawImage( 图像资源，裁剪的x坐标，裁剪的y坐标，裁剪多宽，裁剪多高，x坐标，y坐标, 图像显示的宽，图像显示的高 )
    - 把裁剪到的图像按照指定的大小绘制到指定的位置。
    - 在使用某个img之前，必须监听它onload事件