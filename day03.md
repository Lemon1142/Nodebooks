# 面向对象第三天

### 构造默认的prototype继承结构
- Perdon的显示原型对象，所继承的对象 为object.prototype
- // xiaohong的继承结构：
        var xiaohong = new Person();
 + xiaohong==>Person.prototype==>Object.prototype==>null;

#### obj的继承结构
- obj==> Object.prototype ==> null;

        // obj的继承结构
        // obj ==> Object.prototype ==> null

        /*
        * new Object 创建的实例，继承Object.prototype;
        * 和{}字面量对象一致，
        * 其实{}就是new Object的简写形式。
        * */
        
#### arr的继承结构
- arr ==> Array.prototype ==> Object.prototype ==> null;
         /*
         * new Array 创建的实例，继承Array.prototype;
         * 和[]字面量对象一致，
         * 其实[]就是new Array的简写形式。
         * */

#### Math的继承结构
- Math==> Object.prototype ==>null;

### 继承的规律
1. 谁的实例，这个实例就继承谁的prototype
2. 所有对象继承的终点是Object.prototype
3. 所有函数默认的显示原型 都继承 Object.prototype

