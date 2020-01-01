## let和const：

1. let有哪四点需要注意的？

+ 无变量提升  
+ 需要先声明再使用（存在一个暂时死去）
+ 相同作用域内不能重复声明 
+ 如果外层声明了变量，内层用let重新声明，那么变量属于内层。所以变量使用仍然应在声明之后，否则报错。

2. const和let有哪些异同？如何让const变量的属性也不让变？Object.freeze怎么用？

+ 同：都有作用域、相同作用域内不能重复声明、无变量提升 
+ 异：
  + 1，const声明的数值变量不能变本质是变量指向的内存地址保存的数据不变（数值，对象地址、数组地址）
  + 2，const的声明和赋值必须一起 
+ const声明时，将对象冻结，用Object.freeze，  const foo = Object.freeze({ });  这种冻结在严格模式下生效，一般模式下不生效

3. es6有几种变量声明方法，分别是什么？

   `function var let const import export`   一共6种方法

   字符串扩展：

## 字符串扩展：

1. ES5中，如何用unicode码表示一个字符？在es6中做了什么改进？为什么要做这个改进？
   + es5中，unicode码表示一个字符的方法：\uxxxx 的方式表示一个字符，可以表示\u0000到\uFFFF之间的字符，超过FFFF的只能用双字节来表示

   + es6做的改进，\u{20BB7}

   + 这样做可读取码点超过\uFFFF的字符
2. ES5是以什么格式存储字符的，每个字符多少字节？对于4字节的字符，在ES6中是用哪一个新增的方法处理的？

	+ ：UTF-16 的方式，存储字符，一个字符2字节。

	+ 4字节的字符，es6中codePointAt方法处理

3. codePointAt返回的是码点是多少进制的？如何转化为16进制？

	+ codePointAt返回的码点是十进制

	+ 转化为16进制，toString一下

4. 如何解决codePointAt在使用时如果有4字节字符，码点不对的问题？

+ 用for…of循环

```javascript
let s = '𠮷a';

for (let ch of s) {

  console.log(ch.codePointAt(0).toString(16));

}
```

5. 如何检测一个字符是2字节还是4字节？请写函数is32Bit(c).

```javascript
function is32bit(ch){

    return (ch.codePointAt(0)>  0xFFFF);// 测试有效 

}
```

6. es5如何从码点返回一个对应的字符串？这个方法有什么局限？es6又是如何实现的？

   ①    es5中，码点返回字符串，formCharCode

   ②    局限：不能返回32位的utf-16字符

   ③    es6实现：fromCodePoint实现

7. 简述一下startsWith（param1，param2），endsWith（param1，param2）和includes（param1，param2）用法？

   ①    s.startsWith（str,index） 从字符串s的index位置开始，向后找str，能马上找到返回true，否则false

   ②    s.endsWit(str,index) 从字符串s的index-1位置开始，向前找str，能马上找到返回true，否则false

   ③    s.includes（str,index）从是的index位置开始找，能否找到str，能找到返回true，不能，返回false

8. ES6将字符转化为为码点的方法  codePointAt  ，将码点转化为字符 fromCodePoint 

9. 如何将字符串重复n次，用的是什么函数？此函数的param取正整数，非正整数，字符串，布尔值时，分别表示重复了多少次？

   ①    重复n次：repeat函数。使用方法：str.repeat(count)  str重复count次。

   ②    浮点数时，count会取整；非负数字符串时，count=0；布尔值true时，count=1；布尔false时，count=0 ；负数或负数字符串是时，报错

10. 头部补全和尾部补全，用的是什么方法？用补全的方法，写一个例子，取当天日期，格式是‘yyyy-mm-dd’

    ①    头部补零：s.padStart(len,str)：将字符串s头部用str补充，直到长度是len，返回新字符串

    ②    尾部补零：s.padEnd(len,str)：将字符串s尾部用str补充，直到长度是len，返回新字符串

    ```javascript
    function getToday(){
        var now = new Date();
        let year = now.getFullYear();
        let month = (now.getMonth()+1+'').padStart(2,'0');
        let day = (now.getDate()+'').padStart(2,'0');
        console.log(year+'-'+month+'-'+day);
    }
    ```

11. 反引号是怎么用的？用在什么场合？（两个基本使用方法）`

    场合：换行和赋值  换行时，可在两个反引号中写html代码。赋值时使用美元符+花括号的形式，如：${ param }

12. 数字转化为字符串有哪几种方法？（三种）

     `toString()`方法：  `num.toString()` num为`null`或者`undefined`时，会报错

    `String()`方法: `String(num)`  num为`null`时，转化为`“null”`，为`undefined`时，转化为`“undefined”`

    `num+” ”` : 结果同`String( )`方法

## 正则的扩展：

1.ES5中正则表达式的两种声明方式是什么？分别用两种方式，声明正则表达式 /xyz/ig

①    两种声明方式：直接写表达式var re = /xyz/ig; 等价于var re = new RegExp(/xyz/ig)   ;使用new方法，传入一个字符串和修饰符。var re = new RegExp(‘xyz’,’ig’)；

2.ES6对RegExp函数做了哪些修正，这给正则的声明带来了什么好处？

当RegExp的第一个参数是正则时，可以有第二个参数，且第二个参数会覆盖正则中的修饰符。new RegExp(/xyz/ig,’i’); 的修饰符是u  const r1 = /hello/u r1.unicode

3.ES6用什么修饰符处理码点大于\uFFFF的unicode字符？什么属性可以判断正则是否用了此修饰符？这个修饰符最基本用法是哪些地方（5个），分别给个例子。写一个codePointLength函数返回字符串的真实长度。

①    es6用u修饰符处理码点大于\uFFFF的Unicode字符。

②    unicode属性能判断，是否使用了u字符。用法如下：const r1 = /xy/u;  r1.unicode;结果为true表示使用了u修饰符，为false表示没有。

③    unicode使用的地方：一：点字符（. 识别码点大于0xFFFF时会失效），二：{ }花括号表示法 ，三：量词，四：预定义模式（\S能匹配所有非空字符，码点大于0xFFFF的除外） 五：i修饰符

4.什么是粘性匹配？粘性匹配的修饰符是什么？它和g修饰符的区别在哪里？哪个属性可以判断是否使用了粘性修饰符？

①    粘性匹配：（sticky）必须从剩余的第一个位置开始匹配，修饰符是y。

②    g修饰符是全局匹配，只要剩余位置存在匹配即可。

③    sticky属性可判断粘性修饰符。为true时，存在y修饰符，为false时，不存在y修饰符。

5.lastIndex是什么？在粘性修饰符和g修饰符中有什么用？

①    lastIndex是正则下一次匹配的位置。在y和g修饰符中，都能通过修改lastIndex的值，指定下一次匹配的位置。在粘性模式下，lastIndex开始值是0

6.用粘性修饰符匹配“a1a2a3”，得到[a1,a2,a3]。写出计算过程。

**let** s1 **=** "a1a2a3"**;**

**let** arr **=**  s1**.**match**(****/a\d/gy****);//[a1,a2,a3]**

7.如何返回正则/Abc/gi的正文和修饰符。正文返回的是什么？修饰符返回的又是什么？

①    正文：source属性.  **/Abc/gi.**source  //”Abc”

②    修饰符：flags属性  **let** flags **=** **/Abc/gi.**flags  //’gi’

8.修饰符中，点号有什么局限，如果点号要匹配所有字符，该使用什么修饰符解决？如果正则中用了此修饰符，有什么属性能判断。

①    点号不能匹配码点超过0xFFFF的字符, 用u修饰符解决。点号无法匹配行终止符（断行、回车、行分隔、段分隔符），可用s修饰符解决。

②    如果点号代表一切是，可用dotall属性判断是否在这种模式，返回true或false

/foo.bar/s.dotAll   结果就是true

9.什么是先行断言和后行断言，它们分别的正则表达式是怎样的？分别写正则，识别“100% percent”和“$99 is not cheap”

①    先行断言：x在y前面才匹配，基本js语法就支持。语法格式/x(?=y)/ 

②    后行断言：x在后面，才匹配,es2018引入。写法是：/(?<=y)x/

10.正则表达式如何匹配符合Unicode某种属性的所有字符？如何匹配所用空格和十进制字符？

①    匹配符合unicode属性的所有字符：/\p{propertyName =paropertValue}/

②    匹配所有十进制：**const** regexDecimal **=** **/\p{Decimal_Number}/u;**

③    匹配所有空格：**const** regexSpace **=** **/\p{White_Space}/u;**

11.什么是具名组匹配？这种匹配的写法是怎样的？匹配‘2018-11-02’，返回y，m和d，请尝试用解构赋值的方式获取y，m，d的值。用replace将日期改成2018/11/02

①    具名组匹配：该怎么分就怎么分，分完后，左括号加上？+尖括号+name。例子：const regDate = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/

②    解构赋值，获取y,m,d的值：

​    **const** regDate **=** **/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/****;**

​    **let** str **=** '2018-08-20'

​    **const** matchObj **=** regDate**.**exec**(**str**);**

​    **let** **{**groups**:{**year**,**day**,**month**}}** **=** matchObj**;**

​    console**.**log**(**year**,**month**,**day**);** //2018 08 20

③    replace，将日期转化为2018/08/20模式：需要在字符串替换中引用具名组，方式是$<组名>

**let** transDay **=** str**.**replace**(**regDate**,**'$<year>/$<month>/$<day>'**);**//"2018/08/20"

 

12.正则表达式内，如何使用组名？结合ES5，使用组名的两种方法是什么。匹配’abc!abc!’，进行实例分析。

①    正则表达式内，引用组名，使用\k<组名>

②    使用具名组时，\k<组名>和\1,\2的方式，都行

③    匹配‘abc!abc’的方法：

​    **let** str_t **=** 'abc!abc!ab'**;**

​    **let** reg_twice **=** **/(?<tw>[a-z]{3})!\k<tw>/****;**

​    console**.**log**(**reg_twice**.**test**(**str_t**));** //true

 

13. 遍历器转化为数组的两种方法？

①    Array.from(arrLike,mapFn,thisArg)  

②    ...运算符

## 数值的扩展：

1.二进制和八进制分别如何表示？使用二进制和八进制表示10。如何将二进制和八进制转化为十进制？

①    二进制：0b1010  八进制：0o14  

②    Number，二进制或八进制转化为十进制  Number(0b1010)

2.Number.isFinite( )和Number.isNaN( )如何用，他们和isFinite()和isNaN的差别?

①    Number.isFinite( )是否无限，Number.isNaN( )是否为NaN  

②    Number.isFinite(para )直接判断para满足条件否，isFinite(para)会将para先Number一下，再看是否满足条件。

3.Number,parseInt()、Number.parseFloat()与parseInt()和parseFloat()的区别是什么？

①    ：Number.parseInt()和parseInt（）等价  同理，Number.parseFloat()和parseFloat（）也是等价

4.如何判断一个数是否是正整数？Number.EPSILON是什么？它的应用场景又是什么? 写一下函数acceptError(left,right) 返回值是2的-50次方

①    判断一个数是否正整数：Number.isInteger(para) 

②    Number.EPSILON 是一个常量。2的-52次方。

③    应用场景：浮点数计算的误差范围

function **acceptError**(left,right){

  return Math.**abs**(left - right) < **Number**.EPSILON*Math.**pow**(2,2);

}*//**精度是**2**的**-50**次方*

let resError = **acceptError**(0.1+0.2,0.3);

 

5.js能表示的数的上限和下限是多少？最大上限整数和最小下限整数分别是多少？在上下限之间的安全整数又如何表示？如何验证某个计算结果是安全整数内？

①    JS能表示的下限和上限：2^-53到2^53之间的数  

②    最大上限整数：Number.MAX_SAFE_INTEGER  最小下限整数： Number.MIN_SAFE_INTEGER 

③    上限和下限质检的数：Number.isSafeInteger()

④    验证是否在安全数内：

function **trusty**(left,right,result){

​    if(**Number**.**isSafeInteger**(left)&&

​    **Number**.**isSafeInteger**(right)&&

​    **Number**.**isSafeInteger**(result)

​    ){

return result;

​    }

​    throw new **RangeError**('Operation cannot be trusted!');

} 

 

6.Math.trunc(val)是什么意思？如何判断某数的正、负、0？如何计算一个数的立方根？如何算一个数有多少个先导0？如何计算两数的乘积？如何将64位双精度浮点数转化为32为单精度浮点数？如何求几个数的平方和的平方根？

①    Math.trunc(val)的意思是：去除val的小数部分。运算前，会先把val Number一下

②    判断正负0的方法：Math.sign(val) 运算之前，也会把val Number一下，返回+-1,0

③    计算立方根方法：Math.cbrt(val) 返回val的立方根

④    计算某个数的先导0：Math.clz32(val) 返回先导0的数目

⑤    计算两个数的乘积：Math.imul(num1,num2)  返回乘积的值

⑥    将64位双精度浮点，转化为32位单精度浮点Math.fround 

⑦    几个数的平方和的平方根：Math.hypot(val1,val2,val3) Math.hypot(1,2,2,4)， 返回5

7.如何计算ex-1?如何计算1+x的自然对数？如何返回10为底x的对数?如何返回2为底，x的对数？如何进行指数计算，如2的3次方？

①    计算 ex-1  ：  Math.expm1（x）

②    计算1+x自然对数  ：  Math.log1p(x)

③    计算10位底x对数 ： Math.log10(x) 

④    返回2为底x对数 ： Math.log2(x)

⑤    指数运算符 ** ：console.log(4 ** 3);//64

## 函数的扩展：

1.说一下，函数参数默认值声明时，几点注意事项？（3点）

函数声明注意事项：

①    函数体内，不能用let或const再次声明

②    使用了函数默认值时，不能用重名参数

③    参数的默认值是惰性求值，每调用一次，参数重新计算一次

2.分析一下function m1({x=0,y=0} = {}) { return [x,y]; }和function m2({x,y} = {x:0,y:0}) { return [x,y]; }，传值不同时，分别输出什么?

①    m1和m2都对参数设了默认值。m1的参数默认是{},m2是{x:0,y:0}

②    m1设置了对象的解构赋值，m2没有。

③    当不传值，传{x:0,y:0}或者传其他参数时，效果一样。但是传一个参数时，不一样

3.在函数调用中，传值时，什么样的参数能省略，什么样的参数不能省略？在定义函数时，如何表明函数的某个参数是能省略的？

①    尾部的默认参数能省略

②    非尾部的默认参数不能省略

③    显示传入undefined，触发此参数等于默认值   f(1,undefined,2)

4.函数的length属性时如何计算的？能否包括默认参数和rest属性？

①    length:预期传入的参数个数。要用参数的总个数-默认参数和它后面参数一起，总参数个数

②    不包括默认参数和rest属性

5.函数声明初始化时，什么是作用域？

①    当有默认参数时。函数声明初始化时，会形成单独的作用域。初始化结束，作用域消失。

6.具名函数和赋值的匿名函数，name属性的值是多少？

①    具名函数的name属性是function后的名称

②    赋值的匿名函数，name属性是变量名

③    构造函数，返回的是anonymous

7.箭头函数和解构赋值一起用时，const full =（{first,last}）=>(first + ‘-‘ +last)；full({first：‘chen’,last:’ke’}) 调用结果是啥？

①    调用结果：chen-ke  参数传的是个对象，并且用了最基本的对象解构赋值

8.箭头函数的this指向的是什么？箭头函数自身有没有this？arguments,super,new.target，这些在箭头函数内部存在吗？什么是函数的管道机制？

①    箭头函数this指向：箭头函数定义生效时，所在的对象。箭头函数无this，this指向的是外层变量。

②    arguments，super，new.target在箭头函数内部不存在的。

③    管道机制：前一个函数的输出是后一个函数的输入

9.什么是尾用调用？尾用调用优化的原则是什么？

①    尾用调用：一个函数的最后一步调用了另一个函数。

②    尾用调用的原则：内层函数不再用到外层函数的内部变量时，才能进行尾调用优化

10.用尾递归实现阶乘和斐波拉切的累加和。

①    尾递归实现阶乘：

​    function **factorial**(n,total=1){

if(n == 1) return total;

return **factorial**(n-1,n*total);

​    }

 

②    尾递归实现斐波拉契数列：

function **fb**(n,ac1=1,ac2=1){

if(n <= 1){

​    return ac2;

}

return **fb**(n-1,ac2,ac1+ac2);

​    }

将普通函数改成尾递归，只需要将中间变量改写成函数的参数。

## 数组的扩展：

1.什么是扩展运算符？数组运算符通常的应用场景是？

rest参数：将数字序列转化为数组，常用于给函数传参数。

function **add**(...values){ }

**add**(2,3,4);

扩展运算符：将数组转化为序列，

 

①    扩展运算符：三个点，将数组转化为参数序列，rest参数的逆运算

②    数组运算符应用场景：主要用在函数的调用（参数部分）

2.用扩展运算符求一个数组的最大值。将数组arr1[1,2,3]，加到数组[‘a’,’b’,’c’]的尾部

①    扩展运算符，求数组最大值：

let maxVal = Math.**max**(...[12,45,8,]);

②    将数组arr[1,2,3]，加到数组arr1[‘a’,’b’,’c’]

 let newArr = arr1.**push**(...arr);

3.用ES5的方法实现数组复制。3种方法，用ES6实现数组复制。以上方法是浅拷贝还是深拷贝？如何实现深拷贝？

①    用ES5方法实现数组复制（一维数组）：

​    let arrTwo = [2,3,4,5];

​    let arrTwo2 = arrTwo.**concat**();

 一维数组深拷贝。当arrTwo改变时，arrTwo2没变

②    用ES6实现数组复制：

方法一：

let arrThree = [2,3,4,5];

​    let arrThree1 = [];

​    arrThree1.**push**(...arrThree);

一维数组深拷贝。

方法二：

let [...arrThree2] = arrThree;

一维数组深拷贝。

方法三：

let arrThree3 = [...arrThree]

一维数组深拷贝。

③    实现深拷贝方法：

1， 多维数组深拷贝

​    function **deepCloneArr**(arr){

let out = [],len = arr.length;

for(let i = 0; i < len; i ++){

​    if(arr[i] instanceof **Array**){

out[i] = **deepCloneArr**(arr[i]);

​    }else{

out[i] = arr[i];

​    }

}

return out;

​    }

2， 包含对象数组深拷贝

​    function **deepCloneArrObj**(arr){

let out = [],len = arr.length;

for(let i=0; i < len; i ++){

​    if(arr[i].constructor === **Array**){

out[i] = **deepCloneArr**(arr[i]);

​    }else if(arr[i].constructor === **Object**){

let str = JSON.**stringify**(arr[i]);

out[i] = JSON.**parse**(str);

​    }else{

out[i] = arr[i];

​    }

}

return out;

​    }

测试有效

3， 对象深拷贝方法

​    function **deepCloneObj**(obj){

var newobj;

if(typeof obj != "object"){

​    newobj = obj;

}

else{

​    newobj = obj.constructor == **Array** ? [] : {};

​    var str = JSON.**stringify**(obj);

​    newobj = JSON.**parse**(str);

}

return newobj;}

4.用ES5,ES6的方法，对三个数组arr1，arr2，arr3进行数组合并。

①    ES5实现三个数组合并

let arr = [].**concat**(arr1,arr2,arr3)

 

②    ES6实现三个数组合并

方法一：

let [...arr] = [...arr1,...arr2,...arr3];

  方法二：

  let arrOne = [...arr1,...arr2,...arr3];

 

5.扩展运算符可以和解构赋值相结合，生成数组。这种写法，得到的first，middle，last，分别是多少？const [first, ...middle, last] = [1, 2, 3, 4, 5]，这种写法呢？const [first, ...rest] = [1, 2, 3, 4, 5]

①    扩展运算符和结构赋值相结合

let [first,...last] = [1,2,3,4];*//first: 1  last:[2,3,4]*

 

②    const [first, ...middle, last] = [1,2,3,4,5] 这种写法会报错。因为middle在中间。用结构赋值，给数组进行赋值时，只能放在最后一个参数

③    const [first, ...rest] = [1, 2, 3, 4, 5] *//first: 1  last:[2,3,4,5]*

6.字符串如何转化为数组，并求其长度。

①    字符串转化为数组

方法一：ES5的for … of写法

​    let strFour = 'hello,world';

​    let arrFour = [];

​    for(let i of strFour){

arrFour.**push**(i);

​    }

​    *//["h", "e", "l", "l", "o", ",", "w", "o", "r", "l", "d"]*

方法二：ES6的扩展运算符写法

   let strSix = 'telev';

   let arrSix = [...strSix];  //["t", "e", "l", "e", "v"]

 

②    求字符串长度：   字符串部署了Iterator接口

strSix.length  或者 [...strSix].length

7.字符串如何翻转？用es5和es6的方法实现。两种方法求出来的值都是准确的吗？

①    字符串如何翻转？

方法一：ES5使用了数组的reverse方法，字符串-数组-字符串

​    let strSeven = 'myString';

​    let strSeven2 = strSeven.**split**('').**reverse**().**join**('');

 

方法二：ES6使用了扩展运算符，一步到位

​    let strSeven3 = [...strSeven].**reverse**().**join**('');

 

8.扩展运算符和Iterator的接口有什么关系?请问，如何实现页面上div的数组？

①    扩展运算符和Iterator接口关系：部署了Iterator接口，就能用扩展运算符

②    页面div的数组：

let divs = document.**getElementsByTagName**('div');*//HTMLCollection(6) [div, div, div, div, div, div]*

let divArr = [...divs];

 

9.请定义一个map对象，并遍历它的key和value，用数组形式返回。

①    用map创建一个对象：

​    let mapItera1 = new **Map**([

[5,'one'],

[9,'two'],

[13,'three']

​    ]) //  {5 => "one", 9 => "two", 13 => "three"}

②    数组形式返回，map对象的key

let mapKeys = [...mapItera1.**keys**()];

 

③    数组形式返回，map对象的value

​    let mapValues = [...mapItera1.**values**()];

10.Array.from( )是用来干什么用？它的第二个参数是干嘛用的？写个例子，将类数组[1,,3,,4]中,空出来的数，转化为0

①    Array.from将类似数组或者可遍历对象（有Iterator接口）转化为数组。

看下面几个例子：

例子一：类似数组转化为数组，并进行过滤（数组的filter方法）

​    let div2 = document.**querySelectorAll**('div');

​    let arr2 = **Array**.**from**(div2).**filter**(divEl => {

return divEl.innerHTML.length > 2

​    })

例子二：类似数组转化为数组，同时对数组进行处理（Array.from接受第2个参数）

​    let arrLike = {

0:'a',

1:'b',

2:'c',

length:2*//**必不可少*

​    }

​    let arr3 = **Array**.**from**(arrLike,(item,index) => {

return item+item+index

​    }) // ["aa0", "bb1"]

 

部署了Iterator接口的数据结构都能遍历。目前发现的有这几个：字符串，数组，类似数组的对象，set和map结构

②    Array.from的第二个参数，用来处理转化之后数组中的单个数据

③    将类数组中的空出来的位置，用0补上

​    let arrFour = [1,,2,,3];

​    let arrFour1 = **Array**.**from**(arrFour,(item)=>{

*// return item === undefined ? 0:item;  //**写法一*

return item || 0;*//**写法二*

​    })

 

11.如何将一组值转化为数组（两种方法，ES5和ES6分别一种）？

ES5中有new Array(para)方法，ES6中有array.of(para)方法

无参数，参数个数大于1或参数为空时，两种方法效果相同。参数个数为1时，效果不同。array.of(para)正是为了规避这个，才出现的。

参数个数大于1时：

​    let arrFive = new **Array**(3,12);

​    let arrFive1 = **Array**.**of**(3,12);

*//**都是* *[3, 12]*

​    let arrFive2 = new **Array**();

​    let arrFive3 = **Array**.**of**();

​    *//**都是* *[]*

参数个数为1时：

​    let arrFive4 = new **Array**(3);*//[,,]*

​    let arrFive5 = **Array**.**of**(5);*//[5]*

 

12.copyWithin( ) 方法如何用？详细解释下，它的几个参数。

①    copyWithin( ) 的用法：数组中将指定位置的元素覆盖其他位置的元素。会改变被操作的数组

②    三个参数的含义：

target（必须）：从此处开始替换

start（可选）：被复制的起始位置

end（可选）：被复制的结束位置(复制时，不包括此位置)。复制的字符串长度是end-start

​    let arrSix = ['a','b','c','d','e','f','g'];

​    arrSix.**copyWithin**(2,0);*//["a", "b", "a", "b", "c", "d", "e"]*

  *//**分析：从**arrSix[2]**开始，到后面的元素，被**arrSix**从第一个元素开始覆盖*

 

​    let arrSix1 = ['a','b','c','d','e','f','g'];

​    arrSix1.**copyWithin**(2,3,5);*//["a", "b", "d", "e", "e", "f", "g"]*

 

13.数组的find和findIndex如何用？它们的返回值是多少？可以接收几个参数？

①    find：返回数组中满足条件的第一个数，没有就返回undefined

​    let arrSeven = [10,1,4,12,-6,-20];

​    let itemSeven = arrSeven.**find**(item => {

return item < 0;

​    });*//-6*

find可接收的参数是函数。函数有三个参数.function(currentVal,currentIndex,sourceArr)

②    findIndex:用法和find是一样，传的参数也类似。但是，findIndex返回的是index值。 都不符合，就返回-1

③    find和findIndex都可接收第二个参数。绑定回调函数的this

例子：

​    function **fnV**(v){

return v>this.age;

​    }

​    let person = {name:'John',age:21};

​    [10,13,34,16].**find**(fnV,person);*//34* 

 

14.fill函数如何用?可以接收几个参数，每个参数是什么意思？

①    fill(val,start,end)的用法：按给定的值，将数组指定位置的值进行覆盖

​    let arrFill = new **Array**(3);

​    arrFill.**fill**('a');*//['a','a','a']*

​    arrFill.**fill**('b',1,2);*//['a','b','a']*

  

②    val：必填，填充的值

start：非必填，起始位置，默认是0

end：非必填，结束位置的后一位，默认是数组最后一位的下一位

15.如何遍历数组的key，value，以及key:value键值对。用什么属性，使用这个属性，用2种方式来实现数组的遍历。

①    遍历数组的key：arr.keys()  遍历数组的arr.values() 遍历key：value 值对，arr.entries()

②    这三个属性得到的结果都是一个Iterator对象，要用for…of循环，进行遍历。看例子：

​    let keysArrEight = arrEight.**keys**();

​    for(let item of keysArrEight){

**console**.**log**(item);

​    }

 

16.includes表示什么意思？返回值是什么？可以接收几个参数？

①    includes表示是否包含某个值

②    如果包含，返回true，否则false

③    接受2个参数，第一个是值，第二个是起始搜索的位置，默认是0

## 对象的扩展：

1. 属性和方法的简写方法，分别给出例子。

   属性的简写：直接写变量和函数，作为对象的属性和方法

   ```javascript
   // 直接写变量，已知变量 strOne, strTwo
   const objOne = {strOne, strTwo}
   // 直接写函数，已知函数fnObj返回的是一个对象
   const objTwo = fnObj(1,2)
   ```

   ```javascript
   const o = {
    method(){
   return "hello"
    }
   }
   ```

   

2. 字面量定义对象时，如何用表达式定义属性和方法名？

   ```javascript
   // 定义属性
   let proKey = "kkkk"
   let obj = {
       [proKey]: true
   } //{kkkk: true}
   let objTwo3 = {
       ['h'+'ello'](){
           return 'hi'
       }
   }
   let strThree = objTwo3.hello()
   ```
3. 属性名表达式和简洁表示法能一起使用吗？

   不能

4. 属性表达式如果是对象，结果是多少？

   默认转化为[object object]字符串

   ```javascript
   const keyA = {a: 1};
   const keyB = {b: 2};
   const myObject = {
       [keyA]: 'valueA',
       [keyB]: 'valueB'
   };
   // myObject得到的结果是：
   {[object Object]: "valueB"}
   
   ```


5. 给对象的方法加存值和取值函数是什么意思？例子说明。

   在对象的方法名前加get或set方法( 关于set和get的方法，后续还需要复习一下文档完善 )
   
   ```javascript
   const objThree = {
       get foo(){  },
       set foo(x){  }
   }
   ```

6. ES6里面，严格相等，用什么方法？这个和 === 符号有什么区别？和 == 呢？

   严格相等用Object.is(  )方法

   Object.is( )方法和  ====  的区别主要在于 NaN和 +0，-0

   == 在等的时候会进行类型转换

   ```javascript
   Object.is(parseInt("abc"), parseInt("123"))  //true
   Object.is(+0, -0) // false
   
   parseInt("abc") === parseInt("123") // false
   +0 === -0 // true
   
   "123" == 123
   ```

   

7. Object.assign(  )可以将源对象的所属性复制到目标对象吗？

   不是的。Object.assign(  )只能复制可枚举属性。（什么是可枚举属性？）

   ```javascript
   Object.assign(targetObj, sourceObj1, sourceObj2)
   ```

   除了第一个是目标对象，后面几个都是源对象。

   可枚举属性就是Object.keys（ ）方法可以遍历到的属性。

8. 多个sourceObj中，属性有重名的，怎么处理？

   排在后面的同名属性的值，会覆盖排在前面的，看例子

   ```javascript
   const target = {a:1};
   const source1 = {a:2,b:3,e:6};
   const source2 = {b:4,d:5};
   Object.assign(target,source1,source2) // target是 {a: 2, b: 4, e: 6, d: 5}
   ```

9. 参数只有一个，且是对象时，返回值是什么？

   源对象。

   无论参数是几个，返回值都是源对象。看例子：

   ```javascript
   let res = Object.assign(target,source1,source2)
   res === target //true
   ```

10. Object.assign()对参数的类型有限制吗？拷贝过程有限制吗？

    **参数类型的限制：**

    第一个参数需要能转化为对象。后面的参数不做限制。

    null和undefined不能转化为对象，因此不能作为第一个参数。后面的参数中如果有null或者undefined时，会自动忽略掉。

    非null和undefined的参数，会先转化为对象，再参与运算

    **拷贝过程的限制：**

    只有拷贝源对象自身的属性，即可枚举的属性

11. 字符串可以拷贝到目标对象吗？结合代码说明。

    可以，字符串会先转化为数组，再进行拷贝。

    ```javascript
    let obj_3 = Object.assign({},"chen")  
    // 对象obj_3的值是： {0: "c", 1: "h", 2: "e", 3: "n"}
    ```

12. Object.assign( )是浅拷贝还是深拷贝？

    浅拷贝

13. 同名属性的拷贝是替换还是添加？

    替换。

    下面例子中，objSix3的值被直接替换了。因此，如果不想修改objSix3的值，目标对象最好设为空对象`{}`

    即：`Object.assign({}, objSix3, objSix4)`

    ```javascript
    let objSix3 = {a:{b:1,c:3}};
    let objSix4 = {a:{b:'hello'}};
    let objSix5 = Object.assign(objSix3,objSix4);
    ```
    
14. 可以对两个数组做Object.assign(  )操作吗？

    可以。表现形式时，将数组转为对象，然后进行拷贝。例子如下:

    ```javascript
    let arr1 = [1, 2, 3]
    let arr2 = ["a", "b", "c"]
    let obj = Object.assign({}, arr1, arr2) // {0: "a", 1: "b", 2: "c"}
    ```

15. Object.assign(  )中的源对象中与取值函数时，是怎么处理的？

    如果源对象是个函数，那么先执行函数，再将执行结果插入到目标对象中

    ```javascript
    const sourceSix = {
    	get fooNew(){
	        console.log("get new thing~~")   
            return 1
		}	
    }
    const targetSix = {}
    let objSix7 = Object.assign(targetSix, sourceSix)
    // get fooNew
    // {fooNew: 1}
    ```

16. Object.assign(  )有哪些使用场景，以例子说明

    ①为对象或类添加属性

    ```javascript
    class Point {
        constructor(x,y){
            Object.assign(this, {x, y})
        }
    }
    ```
    ②为对象添加方法: 例如给prototype属性添加方法

    ```javascript
    function Animal(name, age){
        this.name = name
        this.age = age
    }
    Object.assign(Animal.prototype, {
        getName(){
            return this.name
        },
        getAge(){
            return this.age
        }
    })
    let dog = new Animal("lily", 10)
    ```
    ③为对象克隆

    ```javascript
    function clone(origin){
        return Object.assign({}, origin)
    }
    ```
    ④合并多个对象

    ⑤为属性加默认值

    ```javascript
    const Defaults = {
        logLevel: 0,
        outputFormat: "html"
    }
    function processContent(options){
        options = Object.assign({},Defaults,options )
    }
    ```
    
17. 如何获取对象属性的描述对象？

     用Object.getOwnPropertyDescriptor

     `Object.getOwnPropertyDescriptor(objEight,'foo')`

18. 对象`objEight = {foo:123}`， 属性foo的描述对象包含哪4个属性？

     ```javascript
     {value: 123, writable: true, enumerable: true, configurable: true}
     ```

19. `enumerable`是描述对象的什么属性？`Object.keys(  )`跟它有什么关系？

     `enumerable`是可枚举属性。为`true`时表示可枚举，为`false`时表示不可枚举。

     `Object.keys(  )`遍历对象时会忽略掉不可枚举的属性。其他几个和`Object.keys(  )`有相同表现形式的有 `for ...in`,`Object.keys(  )`,`JSON.stringify()`,`Object.assign()`。这四个都会忽略对象 `enumerable`为 `false`的属性。

     `for...in`会返回继承的可枚举属性。`Object.keys(  )`不会返回，所以获取自身的可枚举属性，用`Object.keys(  )`。看下面的例子：

     ```javascript
     let obj = {name: "xiaokeai"}
     let targetObj = Object.create(obj)
     targetObj.age = 30 // name是继承的属性,age是自身的属性
     console.log( Object.keys(targetObj)) // ["age"] ,  Object.keys只能获取自身属性
     for(let key in targetObj){
     console.log(key, targetObj[key]) // for ... in还可以获取继承属性
     }
     // 
     ```

     `obj.hasOwnProperty(key)`只能判断属性key是不是属于obj自身，自身有时，才返回true。其他情况，返回false，所以继承的属性，返回的是false。看mdn上的例子：

     ```JavaScript
     var triangle = {a: 1, b: 2, c: 3};
     
     function ColoredTriangle() {
       this.color = 'red';
     }
     
     ColoredTriangle.prototype = triangle;
     
     var obj = new ColoredTriangle();
     obj.hasOwnProperty("color") // true  其他情况下都是false
     ```

     

20. 属性有哪些遍历方法？

     四种方法。

     ①   Object.keys(  )

     ②   for ... in // 继承属性也会遍历出来

     ③   Object.getOwnPropertyNames(obj)  // 和Object.keys(  )结果一样

     ④   Object.getOwnPropertySymbols(obj)  //是 [ ]

     ⑤   Reflect.ownKeys(obj)  // 和Object.keys(  )结果一样

     

21. 属性的遍历规则是什么？（从数值，字符串，`Symbol`考虑）

     数值键： 按升序排列

     字符串 ：按加入时间升序

     `Symbol`键：按照加入时间升序

22. `Object.getOwnPropertyDescriptors()`是用来干嘛？

     解决Object.assign（ ）无法正确拷贝set和get属性的问题

     ```JavaScript
     let objNine2 = Object.getOwnPropertyDescriptors(objNine)
     ```

23. 如何设置和读取对象的prototype对象？如何将已有对象为原型，创建新对象？

     设置对象的原型对象： `Object.setPrototypeOf({  })`

     获取对象的原型对象：`Object.getPrototypeOf( {  } )`

     以新对象为原型对象，创建新对象： `Object.create(obj)`

24. 用字面量定义一个空对象和用Object.create(null)定义一个空对象，有什么区别？

     用字面量定义的空对象，有 `__proto__`属性，指向的是它的原型对象（此时是构造函数 `Object`的Prototype属性）。有原型对象的属性和方法，例如： `hasOwnProperty`， `toString`等 方法

     用 `Object.create(null)`，因为原型对象是null，所以没有属性和方法可以继承，因此，得到的是一个干干净净的对象，没有任何属性。

25. 什么关键字指向的是当前对象的原型对象？

     super关键字。看下面的例子：

     ```javascript
     const proto11 = {
     foo : "hello"
     }
     const obj11 = {
     foo: "world",
     find(){
     return super.foo //原型对象的foo属性，即 hello
     }
     }
     Object.setPrototype(obj11,proto11)
     obj11.find() // "hello"
     ```

     super作为关键字时，只能放在对象的方法里。用在属性里就会报错。

26. Object.keys( )遍历的对象属性有什么特点？

     都是可遍历属性；不含继承属性；都会过滤掉`Symbol`值的属性

27. Object.keys()， Object.values()和Object.entries()返回值分别是什么？给出他们三个和解构赋值，for...of一起用的例子？

     ```javascript
     let { keys, values, entries } = Object
     for(let key of entries(obj)){
     consle.log(key)
     }
     ```

     

28. ``Object.entries()`有哪两种用途？

     1. 遍历对象，得到`[key, value]`键值对组成的数组

29. 将对象转化为真正的map解构。看例子：

     ```javascript
           const obj = {foo: "bar", baz:42}
           const map = new Map(Object.entries(obj))
     ```

30. 对象中可以用扩展运算符吗？

     可以用。但是扩展运算符只能用在尾部。看例子：

     ```javascript
     let { x13, y13, ...z13} = { x13: 1, y13: 2, a13: "c", d13: "b"}  
     // z13的值是：{a13: "c", d13: "b"}
     ```

     

31. 扩展运算符能不能继承原型对象的属性？

     不行。扩展运算符只能复制对象自身的属性，不能复制继承属性。例如下面代码中，obj有b属性，但是targetObj没有。因为b属性是继承自obj的原型对象proObj。

     ```javascript
     let obj = {a: 1}
     let proObj = {b: 2}
     Object.setPrototypeOf(obj, proObj)
     let {...targetObj} = obj
     obj.b  //2
     targetObj.b // undefined
     ```

## Symbol实现：

1. `Symbol`解决了什么问题？

   保证每个属性名的独一无二，从根本上防止属性名的冲突

2. JavaScript语言里有哪几种数据类型？

   undefined  null  布尔值  字符串  数值  对象 

3. `Symbol`值如何生成？ 

   通过`Symbol`函数生成。

4. 对象的属性名有哪几种类型？

   两种类型，字符串类型和`Symbol`类型。

5. `Symbol`函数前能够加new运算符吗？

   不行。因为生成的`Symbol`是基础类型，用new运算符得到的是对象。

6. 函数`Symbol()`生成的`Symbol`实例能添加属性吗？

   不能，原因也和上一条一样。

7. Symbol的参数是什么？Symbol的传参有什么作用？

   可以接收一个字符串作为参数。例如 `Symbol("foo")`

   参数是对`Symbol`的描述，传参是为了控制台显示，或者转化为字符串时，比较容易区分。

   ```javascript
   // 对一个Symbol类型的值，转化为字符串
   let s1 = Symbol("nice")
   // 结果是 "Symbol(nice)"
   s1.toString()  
   ```

8. 相同参数的`Symbol`函数的返回值相等吗？

   不相等。参数只是对`Symbol`值的描述。`Symbol`函数的返回值都是独一无二的。

   ```javascript
   let s1 = Symbol("xiaoke")
   let s2 = Symbol("xiaoke")
   s1 === s2   //  false
   ```

9. `Symbol`值可以进行运算吗？

   不可以。`Symbol`参与运算会报错。

10. `Symbol`可以转化为哪些数据类型？

    `Symbol`类型的值，可以转化为字符串和布尔值

    转化成字符串

    ```javascript
    let sym = Symbol("my symbol")
    String(sym)
    //或者 sym.toString() ， 得到的结果都是： "Symbol(My symbol)""
    ```

    转化成布尔值

    ```javascript
    let sym = Symbol()
    let flag = !!sym // 得到的结果是true
    ```

11. 获取`Symbol`的描述

    ```javascript
    const sym = Symbol("foo")
    let str = sym.description // "foo"
    ```

12. `Symbol`作为属性名如何用？

    `Symbol`作为属性名时，定义方法有两种：

    + 写在方括号里面

    ```javascript
    // 用方括号
    let mySymbol = Symbol()
    let a = {
        [mySymbol]: "hello"
    }
    ```

    + 用 `defineProperty` 定义

    ```javascript
    let mySymbol = Symbol()
    Object.defineProperty(a, mySymbol, {value: "Hello!"})
    ```

    取`Symbol`属性时，只能用方括号，不能用点号，因为点号后面永远是字符串

    ```javascript
    let a = {[mySymbol]: "Hello"}
    a.mySymbol  // undefined
    a[mySymbol] // "Hello"
    ```

13. `Symbol`定义一组常量的例子

    ```javascript
    const COLOR_RED    = Symbol();
    const COLOR_GREEN  = Symbol();
    
    function getComplement(color) {
      switch (color) {
        case COLOR_RED:
          return COLOR_GREEN;
        case COLOR_GREEN:
          return COLOR_RED;
        default:
          throw new Error('Undefined color');
        }
    }
    ```

14. `Symbol`值作为属性名时，该属性是私有属性还是公开属性？

    公开属性。

15. `Symbol`值用来消除魔术变量的例子：

    ```javascript
    const shapeType = {
        triangle: Symbol();
    }
    function getArea(shape, options){
        let area = 0;
        switch(shape){
            case shapeType.triangle:
                area = .5 * options.width * options.height
                break;
        }
        return area;
    }
    ```

16. 如何获取对象的所有`Symbol`属性名？

    用 `Object.getOwnPropertySymbols()`方法，可以获取对象中国所有的`Symbol`属性名。

    该方法返回一个数组，成员是当前对象的所有用作属性名的`Symbol`值。

17. 如何返回所有类型的键名？【包括常规键名和`Symbol`键名】

    `Reflect.ownKeys()`返回常规键名和 `Symbol`键名

18. 如何给对象定义一些非私有，但又希望只用于内部的方法？

    利用`Symbol`值作为键名不会被常规方法遍历到的特点。

19. `Symbol.for()`使用场景是什么？

    重新使用同一个 `Symbol`值。

20. `Symbol.for()`是如何用的？

    `Symbol.for()`接收一个字符串作为参数。然后搜索有没有该参数作为名称的`Symbol`值。如果有，就返回这个`Symbol`值，如果没有，就新建一个该字符串为名称的`Symbol`值，并注册到全局。

    有三层意思：

    + 新旧的`Symbol`值，必须全部是`Symbol.for()`方法生成，才可能相等
    + 由`Symbol.for()`生成的`Symbol`值，因为注册在全局，因此两个`Symbol`相等，可以发生在不同作用域里 
    + 新旧的`Symbol`值，如果全部用`Symbol()`函数生成，则永远不可能相等

21. 如何获取一个已登记的 `Symbol` 类型值的key？

    `Symbol.key()`方法

    ```javascript
    let s1 = Symbol("symbol test")
    let key1 = Symbol.keyFor(s1)
    
    let s2 = Symbol.for("symbol test")
    let key2 = Symbol.keyFor(s2)
    ```

    key1和key2 分别是 undefined和“symbol test”

22. 什么是模块的 `Singleton` 模式？

    调用一个类，任何时候返回的都是同一个实例。

    非`Symbol`的方式实现：思路是把实例放到顶层对象global

    ```javascript
    function A() {
      this.foo = 'hello';
    }
    
    if (!global._foo) {
      global._foo = new A();
    }
    
    module.exports = global._foo;
    ```

    `Symbol`方式实现：思路是把实例

    ```javascript
    const FOO_KEY = Symbol.for('foo');
    
    function A() {
      this.foo = 'hello';
    }
    
    if (!global[FOO_KEY]) {
      global[FOO_KEY] = new A();
    }
    
    module.exports = global[FOO_KEY];
    ```

    

23. 内置的`Symbol`值，目前仅做了解

    `Symbol.hasInstance`，`Symbol.isConcatSpreadable`，`Symbol.species`，`Symbol.match`  `Symbol.replace`  `Symbol.search`   `Symbol.split`
    `Symbol.iterator`   `Symbol.toPrimitive`   `Symbol.toStringTag`  `Symbol.unscopables`

## Proxy

1. 如何理解 Proxy？

   修改某些操作的默认行为。在目标对象之前架设一层“拦截”，外界对该对象的访问，必须要先通过这层拦截。

2. 如何生成Proxy实例？

   `Proxy`对象的所有用法，都是下面这种形式。不同的只是handler参数的写法。

   ```javascript
   let proxy = new Proxy(target, handler)
   ```

   `target`参数表示所要拦截的目标对象， `handler`参数也是对象，用来定制拦截行为。

   拦截读取属性的例子：

   ```javascript
   let proxy = new Proxy({}, {
       get(target, propKey){
           return 35;
       }
   });
   // proxy.time => 35
   // proxy.name => 35
   ```

   

3. 如何让`Proxy`生效？

   针对`Proxy`实例进行操作，而不是目标对象进行操作。

4. `Proxy`实例作为其他对象的原型对象？

   ```javascript
   let proxy =new Proxy({}, {
   	get(target, propKey){
           return 35;
       }
   })
   let obj = Object.create(proxy)
   obj.time // 35
   ```

   proxy对象是obj对象的原型对象，obj上没有time属性，根据原型链，会在proxy对象上读取，导致被拦截。

5. 同一拦截器函数，可以设置拦截多个操作吗？

   同一拦截器函数可以设置拦截多个操作。

6. 对于可以设置，但是没有设置拦截的操作，会怎样？

   按照原先（没拦截时）的方式，产生结果。

7. Proxy实例的get方法的作用是什么？如何传参的？

   作用： 拦截对象属性的读取，比如 proxy.foo 和 proxy["foo"]

   传参：get（target， propKey， receiver） target是目标对象， propKey是访问的属性，receiver指向的是Proxy实例。

   ```javascript
   var person = {
     name: "张三"
   };
   var proxy = new Proxy(person, {
     get: function(target, propKey) {
       console.log(target == person);
       console.log(receiver === proxy);  
       if (propKey in target) {
         return target[propKey];
       } else {
         throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
       }
     }
   });
   proxy.name // "张三"
   proxy.age // 抛出一个错误
   ```

8. Proxy不能修改什么样的属性？

   如果一个属性不可配置（confgurable）且不可写（writable），则Proxy不能修改该属性且通过Proxy对象访问该属性会报错。

9. Proxy实例的set方法的作用是什么？如何传参的？

   set方法用来拦截某个属性的赋值操作。

   set方法接收四个参数，依次是目标对象、属性名、属性值和Proxy实例本身，最后一个参数可选。

10. Proxy实例的set方法的例子

    + 设置属性的最大值不超过200

      ```javascript
      let validator = {
          set(obj, prop, value){
              if(prop === "age" && (Numberi.isInteger(value) || value > 200)){
                  throw new TypeError("The age seems invalid")
              }
              obj[prop] = value
          }
      }
      ```

    + 对象发生变化时，自动更新dom

    + 防止下划线开头的内部属性，被外部读写

11. set方法对什么样的属性不起作用?

    如果目标对象自身的某个属性，不可写且不可配置，那么set方法将不起作用

    ```javascript
    const obj = {}
    Object.defineProperty(obj, "foo", {
        value: "bar",
        writable: false
    })
    const handler = {
        set(obj, prop, value, receiver){
            obj[prop] = "baz"
        }
    }
    const proxy = new Proxy(obj, handler);
    proxy.foo = "baz"
    proxy.foo // bar
    ```

12. 在严格模式下，如果set代理返回false或者undefined，会怎样？

    会报错。

13. apply方法有什么作用？

    apply方法拦截`函数`的调用、call和apply操作。

    apply方法可以接收三个参数，目标对象、目标对象上下文（this）和目标对象参数数组。

    ```javascript
    let handler = {
    	apply(target, ctx, args){
            return Reflect.apply(...arguments)
        }
    }
    ```

14. has()方法有什么用？使用场景是什么？

    has方法用来拦截HasProperty操作。当 `return false`时，表示拦截成功。

    典型的操作是in运算符的时候 。

    看例子：

    ```javascript
    let handler = {
    	has(target, key){
            if(key[0] === "_"){
                return false  // 对_开头的属性拦截成功
            }
            return key in target
        }
    }
    let target = {_prop: "foo", prop: "foo"}
    let proxy = new Proxy(target, handler)
    "_prop" in proxy    // false
    ```

15. has拦截什么时候会报错？

    如果目标对象不可扩展，或者某个属性不可配置，则has拦截就会报错。

16. has方法拦截的是 `hasProperty`还是 `hasOwnProperty`?对 `for...in`有效吗？

    拦截的是 `hasProperty`

    has拦截对 `for...in`循环不生效

17. construct方法有什么用？接收几个参数？

    `construct`方法，用于拦截new命令。

    接收三个参数。target  【目标对象】, args【构造函数的参数对象】, newTarget【创造实例对象时，new命令作用的构造函数】

18. deleteProperty的使用场景是什么？

    deleteProperty用于拦截delete操作。如果这个方法抛出错误或者返回false，则当前属性无法被delete命令删除。

19. deleteProperty方法什么时候会报错？

    目标对象自身的不可配置属性，不能被deleteProperty方法拦截，否则会报错。

20. defnieProperty的使用场景是什么？

    `defineProperty`方法 拦截 `Object.defineProperty`操作。

    当 `defineProperty`返回的是fanlse时，拦截生效。

21. getOwnPropertyDescriptor方法的使用场景是什么？

    getOwnPropertyDescriptor方法拦截 `Object.getOwnPropertyDescriptor()`，返回一个属性的描述对象或者undefined

22. getPrototypeOf 方法的使用场景是什么？

    拦截获取原型对象，返回值是对象或者null，否则报错。

    拦截的操作包括：

    ```javascript
    Object.prototype.__proto__
    Object.prottype.isPrototypeOf()
    Object.getPrototypeOf()
    Reflect.getPrototypeOf()
    instanceof
    ```

23. isExtesible（）方法的作用是什么？返回值是什么

    isExtensible方法拦截Object.isExtensible操作，返回的是布尔值。

    返回值必须与目标对象的isExtensible属性保持一致，否则会抛出错误。

24. ownKeys()方法的作用是什么？

    拦截对象自身属性的读取操作。拦截的操作有：

    `Object.getOwnPropertyNames()`

    `Object.getOwnpropertySymbols()`

    `Object.keys()`

    `for...in` 

25. ownKeys()方法的返回值是什么？

    是数组。准确说是所有可能`属性名`组成的数组。

26. 使用 `Object.keys()`方法时，有哪三类属性会被 `ownKeys`方法自动过滤？

    + 目标对象不存在的属性
    + 属性名为Symbol的属性
    + 不可遍历（enumerable）的属性

    ```javascript
    let target = { a: 1, b: 2, c: 3, [Symbol.for('secret')]: '4'};
    Object.defineProperty(target, 'key', {
      enumerable: false,
      configurable: true,
      writable: true,
      value: 'static'
    });
    
    let handler = {
      ownKeys(target) {
        return ['a', 'd', Symbol.for('secret'), 'key'];
      }
    };
    let proxy = new Proxy(target, handler);
    Object.keys(proxy)
    // ['a']
    ```

    不存在的属性（d）、Symbol 值（Symbol.for('secret')）、不可遍历的属性（key），都被自动过滤掉了。

27. `ownKeys`在使用时有哪些注意的点？

    + ownKeys方法返回的数组成员，只能是字符串或 Symbol 值。如果有其他类型的值，或者返回的根本不是数组，就会报错
    + 如果目标对象自身包含不可配置的属性，则该属性必须被ownKeys方法返回，否则报错
    + 如果目标对象是不可扩展的（non-extensible），这时ownKeys方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性，否则报错

28. preventExtensions方法的作用是什么？返回值是什么？

    拦截 `Object.preventExtensions()`。

    返回值是布尔值，如果不是，则会自动转化为布尔值。

29. 使用时有哪些注意的点？

    只有目标对象不可扩展时（即`Object.isExtensible(proxy)`为`false`），`proxy.preventExtensions`才能返回`true`，否则会报错。

    所以，通常要在`proxy.preventExtensions`方法里面，调用一次`Object.preventExtensions`。

30. `setPrototypeOf`方法的作用是什么？返回值什么？

    作用是： 拦截 `Object.setPrototype`方法。

    返回值是布尔值，如果不是布尔值，则自动转化为布尔值

31. Proxy.revocable()方法的作用是什么？

    Proxy.revocable()方法返回一个可取消的Proxy实例。

    ```javascript
    let target = {};
    let handler = {};
    let {proxy, revoke} = Proxy.revocable(target, handler);
    proxy.foo = 123;
    proxy.foo // 123
    revoke(); // 访问结束后，就收回代理权，不允许再次访问
    proxy.foo // TypeError: Revoked
    ```

32. Proxy代理的情况下，目标对象内部的this指向的是什么？

    指向的是 Proxy 代理。

     有些原生对象的内部属性，只有通过正确的`this`才能拿到，所以 Proxy 也无法代理这些原生对象的属性。 

    ```javascript
    const target = new Date();
    const handler = {};
    const proxy = new Proxy(target, handler);
    proxy.getDate(); // 报错
    ```

## Reflect

1. `Reflect`对象设计的目的有哪些？

   + 将 `Object`对象的一些明显属于语言内部的方法，放到`Reflect`对象上

   + 修改`Object`方法的返回结果，使其变得更合理。

   + 让Object操作变为函数行为。比如 for...in  和 delete方法

     ```javascript
     name in obj => Reflect.has(obj, name)
     delete obj[name] => Reflect.deleteProperty(obj,name)
     ```

   + Reflect对象的方法与proxy对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。 不管`Proxy`怎么修改默认行为，总能在`Reflect`上获取默认行为。

2. `Reflect`对象有哪13个静态方法？

   ```javascript
   Reflect.apply(target, thisArg, args)
   Reflect.construct(target, args)
   Reflect.get(target, name, receiver)
   Reflect.set(target, name, value, receiver)
   Reflect.defineProperty(target, name, desc)
   Reflect.deleteProperty(target, name)
   Reflect.has(target, name)
   Reflect.ownKeys(target)
   Reflect.isExtensible(target)
   Reflect.preventExtensions(target)
   Reflect.getOwnPropertyDescriptor(target, name)
   Reflect.getPrototypeOf(target)
   Reflect.setPrototypeOf(target, prototype)
   ```

   大部分和`Object`对象的同名方法的作用是相同的，而且它与`Proxy`对象的方法是一一对应的。

3. `Reflect.get`方法查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`。

   ```javascript
   var myObject = {
     foo: 1,
     bar: 2,
     get baz() {
       return this.foo + this.bar; // 如果有get，里面的this是第三个参数receiver
     },
   };
   
   var myReceiverObject = {
     foo: 4,
     bar: 4,
   };
   
   Reflect.get(myObject, 'baz', myReceiverObject) // 8
   ```

4. `Reflect.set`方法 设置 `target`对象的 `name`属性等于 `value`

   ```javascript
   var myObject = {
     foo: 4,
     set bar(value) {
       return this.foo = value; // 如果name属性设置了赋值函数，则赋值函数的this绑定receiver，此时是 myReceiverObject
     },
   };
   
   var myReceiverObject = {
     foo: 0,
   };
   
   Reflect.set(myObject, 'bar', 1, myReceiverObject);
   myObject.foo // 4
   myReceiverObject.foo // 1
   ```

5. 如果`Proxy`对象和`Reflect`对象联合使用，而且`Reflect`操作完成了赋值的默认行为，而且传入了receiver，则`Reflect.set`会触发`Proxy.defineProperty`拦截。为什么呢？

   这是因为`Proxy.set`的`receiver`参数总是指向当前的 `Proxy`实例（即上例的obj），而`Reflect.set`一旦传入`receiver`，就会将属性赋值到`receiver`上面（即obj），导致触发`defineProperty`拦截。如果`Reflect.set`没有传入`receiver`，那么就不会触发`defineProperty`拦截。

6. `Reflect.has(obj, name)`的用法是什么？

   `Reflect.has`对应 `name in obj`里的`in`运算

   ```javascript
   var myObj = {
       foo: 1
   }
   Reflect.has(myObj, "foo") // true
   ```

7. `Reflect.deleteProperty(obj, name)`的用法是什么？

   对应`delete myObj.foo`，如果删除成功，或者不存在，则返回true，否则返回false

   ```javascript
   Reflect.deleteProperty(myObj, "foo")
   ```

8. `Reflect.construct(targetFn, args)`的用法是什么？

   等同于 `new target(...args)`，不使用new来调用构造函数的方法。

   ```javascript
   function Greeting(name){
   	this.name = name;
   }
   const instance = new Greeting("张三")
   // or
   cost instance = Reflect.construct(Greeting, ["张三"])
   // 第一个参数是函数，第二个参数是 参数数组
   ```

9. `Reflect.getPrototypeOf`的作用是什么？

   `Reflect.getPrototypeOf`方法用于读取对象的__proto__属性，对应 `Object.getPrototypeOf(obj)`.。区别是，当参数param不是对象时，`Object.getPrototypeOf(param)`会将param转化为对象，而 `Reflect.getPrototypeOf(param)`会报错。

   ```javascript
   const myObj = new FancyThing()
   Object.getPtototypeOf(myObj) === FancyThing.prototype   // true
   Reflect.getPrototypeOf(myObj) === FancyThing.prototype  // true
   ```

10. `Reflect.setPrototypeOf(obj, newProto)`的用法

    设置目标对象的原型。返回一个布尔值，表示是否设置成功。和`Object.setPrototypeOf(obj，newProto)`相对应。

    ```javascript
    // 旧写法
    Object.setPrototypeOf(myObj, Array.prototype);
    // 新写法
    Reflect.setPrototypeOf(myObj, Array.prototype);
    ```

    如果第一个参数是 undefined或者 null，Object.setPrototypeOf和Reflect.setPrototypeOf都会报错

    除开第一种情况，如果第一个参数不是对象，则Object.setPrototypeOf会返回第一个参数本身，而Reflect.setPrototypeOf会报错。

11. `Reflect.apply(func, thisArg, args)`的用法？

    用于绑定this对象后执行给定函数。

    等价于`Function.prototype.apply.call(func, thisArg, args)`

    看例子：

    ```javascript
    const ages = [11,33,12,5,18,96]
    const youngest = Reflect.apply(Math.min, Math, ages)  // 5
    ```

12. `Reflect.defineProperty(target, proppertyKey, attributes)`的用法？

    此方法等同于`Object.defineProperty` ，用来为对象定义属性。它和 `Proxy.defineProperty` 配合使用的例子：

    ```javascript
    const p = new Proxy({}, {
        defineProperty(target, prop, descriptor){
            console.log(descriptor)
            descriptor.value = "bar-haha"
            return Reflect.defineProperty(target, prop, descriptor)
        }
    })
    // {value: "bar", writable: true, enumerable: true, configurable: true}
    p.foo = "bar"
    // bar-haha
    console.log(p.foo)
    ```

13. `Reflect.getOwnPropertyDescrptor(target, proppertyKey)`的用法？

    等同于 `Object.getOwnPropertyDescriptor`用于得到指定属性的描述对象.

    区别是如果`target`参数不是对象， `Object.getOwnPropertyDescriptor`  不报错，发挥undefined，而`Reflect.getOwnPropertyDescrptor`会报错。

    ```
    var myObject = {};
    Object.defineProperty(myObject, 'hidden', {
      value: true,
      enumerable: false,
    });
    var theDescriptor = Reflect.getOwnPropertyDescriptor(myObject, 'hidden');
    ```

14. `Reflect.isExtensible(target)`的用法？

    `Reflect.isExtensible(target)`方法对应 ` Object.isExtensible `，返回一个布尔值，表示对象是否可扩展。

15. ` Reflect.preventExtensions (target)`的用法？

     `Reflect.preventExtensions`对应 `Object.preventExtensions`方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。 

    ```javascript
    var myObject = {};
    Reflect.preventExtensions(myObject) // true
    ```

16. `Reflect.ownKeys(target)`的用法？

    方法用于返回对象的所有属性，等同于  `Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`之和 。

    ```javascript
    var myObject = {
        foo: 1,
        bar: 2,
        [Symbol.for("baz")]: 3,
        [Symbol.for("bing")]: 4
    }
    Reflect.ownKeys(myObject)  // ["foo", "bar", Symbol(baz), Symbol(bing)]
    ```

17. 什么是观察者模式？

    函数自动观察数据对象，一旦对象有变化，函数自动执行。

    使用`Proxy`写一个观察者模式的例子：

    ```javascript
    const queuedObservers = new Set();
    
    const observe = fn => queuedObservers.add(fn);
    const observable = obj => new Proxy(obj, {set});
    
    function set(target, key, value, receiver) {
      const result = Reflect.set(target, key, value, receiver);
      queuedObservers.forEach(observer => observer());
      return result;
    }
    ```

## set和map:

1. Set是什么？

   Set是一个构造函数。可以用new Set( )用于生成Set数据结构。

2. Set结构有什么特点？

   不会添加重复的值。

3. 如何向Set结构中添加成员？

   通过add方法，向Set结构的实例中添加成员。

   ```javascript
   let set = new Set()
   [2,3,4,5,5,1,2].forEach(item=>set.add(item))
   // s是  Set(5) {2, 3, 4, 5, 1}
   ```

   

4. 如何遍历Set实例？

   两种方法： for...of方法遍历；扩展运算符遍历。看例子：

   （可以用for ... of遍历的地方，就能用扩展运算符）

   ```javascript
   for(let i of set){
     console.log(i) // 依次打印 2,3,4,5,1
   }
   let list = [...set]  // 结果是数组： [2, 3, 4, 5, 1]
   ```

   

5. Set结构如何初始化？

   传数组

   ```javascript
   const set = new Set([3,2,1,2,2,1])
   set.size// 3
   ```

   传类数组：

   ```javascript
   let divs = document.querySelectorAll("div")
   const set = new Set(divs)
   ```

   

6. 给数组去重的方法？

   set方法

   ```javascript
   let arr1 = [2,1,1,5,2]
   let arr2 = [...new Set(arr1)]
   // arr2 [2, 1, 5]
   ```

   上面，计算arr2时用扩展运算符或者用`Array.from()`都可以

   `Array.from(new Set(arr1))`

7. set实例中加入数据时，会对数据进行类型转换吗？

   加入数据时，不会发生类型转换。

8. Set实例内部判断相等时，判断的条件是什么？

   类似于===，但是NaN等于自身。也就是跟Object.is( )类似

   加对象时，比的是对象的地址。所以，两个相同元素的对象，不相等，会add两次。

   ```javascript
   let set = new Set()
   set.add({})
   set.add({})
   set.size // 2
   ```

   

9. Set的实例set（下文中的set都是代指Set实例）有哪些属性？

   `set.prototype.constructor` 指向的是构造函数，默认的是Set构造函数

   `set.prototype.size`返回的是s的成员总数

10. Set的实例set有哪些方法？

    `add(value)`给s添加成员，返回s实例

    `delete(value)`删除成员`value`，返回的是布尔值，表示删除是否成功

    `has(value)`返回布尔值，表示是否有成员`value`。有的话是true，没有是false

    `clear()`清除所有成员，无返回值

    ```javascript
    let s = new Set()
    let arr = ["kk","172", "female"]
    arr.forEach(item => {
s.add(item)
    })
    s.has("172") // true
    s.delete("172") // 删除“172”选项
    s.clear() // 清空所有成员，得到的是一个空set结构
    ```

    

11. Set实例可以用链式写法吗？为嘛？哪些可以用？

    部分可以。`add`方法可以用，因为方法的返回值是新的实例`s`。看例子：

    ```javascript
    s.add(2).add(34)  // Set(2) {2, 34}
    ```

12. 判断是否含某个键值时，对象和s分别怎么判断？

    对象直接用 `obj[property]`

    s用has方法，`s.has(property)`

13. 数组去重可以怎么做？

    `Array.from`和 `set`结合使用，看例子:

    ```javascript
    // Set构造函数可以传数组，也可以传类数组
    // 有重复元素的数组传进去，得到的是去重后的set结构
    // set结构，经过Array.from（）的处理转化为真正的数组
    Array.from(new Set([1,2,3,4,1,2,3]))  //[1, 2, 3, 4]
    // 注意，Array.from()可以传2个参数，第二个参数是对得到的数组做map操作
    ```

    

14. `set`有哪些遍历方法？

    `keys()` 返回键名遍历器
    
    `values()` 返回键值遍历器 
    
    `entries()` 返回键值对遍历器
    
    `forEach()`方法，访问到每个 成员，在回调里，对成员进行操作
    
    遍历器可以用for...of访问，因此要获取 `keys()` `values()` 和 `entries()` 的结果，可以用 `for...of `     

15. Set的实例可以遍历吗？如何遍历？

    Set实例都能遍历，用 `for...of...`方法。

16. Set结构的默认遍历器生成函数是什么？

    Set结构的 `values`方法

    `Set.prototype[`Symbol`.iterator] === Set.prototype.values`  // true

17. Set有哪些使用场景（四点）?

    1. 用扩展运算符，将Set变为数组
    2. 用map和filter等数组方法，间接作用于Set结构
    3. 使用Set的方法，做并集（根据Set结构的不重复特性）、交集（数组的filter和set的has 方法）、差集（数组的filter和set的has方法）
    4. 遍历set时，改变set（将新的set赋值给原来的set  或者  用Array.from方法，对第一个和第二个参数进行处理 )

18. WeakSet是什么？

    WeakSet是构造函数，接收素组或者类数组（Iterator 接口都行）

19. WeakSet结构有什么特点?

    1. 成员只能是对象
    2. 成员对象是弱引用，不计入js的垃圾回收机制。当其他对象不再引用此对象时，垃圾回收机制会回收该对象占用的内存
    3. WeakSet实例不可遍历（原因是上条，WeakSet的成员不稳定，会随后消失)

20. WeakSet实例有哪些遍历方法？

    ```javascript
        WeakSet.prototype.add(obj) // 添加obj
        WeakSet.prototype.delete(obj) // 删除obj
        WeakSet.prototype.has(obj) // obj是否在实例中
    ```

    

21. WeakSet实现： 限制一个类的某个方法，只能在实例中使用

    ```javascript
        const foos = new WeakSet()
        class Foo {
          constructor() {
            foos.add(this)
          }
          method() {
            if (!foos.has(this)) {
              throw new TypeError("Foo.prototype.method 只能在Foo实例上调用！")
            } else {
              console.log("找到组织啦，哈哈~~~");
            }
          }
        }
    
        let foo = new Foo()
        foo.method()  
        Foo.prototype.method()
    // this是谁调用指向谁。第一个this指向的是Foo的实例foo；第二个this指向的是Foo.prototype对象
    ```

    

22. Map作为构造函数，Map实例和对象有什么区别？

    Map生成的实例是 值-值 的形式；对象是 字符串-值 的形式。对象只能把字符串作为键值，而Map不限于字符串，各种类型的值都可以作为键值（包括对象）

    珂珂的理解：Map是对象的补充

23. Map的实例map（下文中的map均指代如此），有哪些方法？

    存值： `set(key, value)`

    取值： `get(key)`

    判断：`has(key)`

    删除：`delete(key)`

    清空： `clear()`

    ```javascript
        const m = new Map()
        const o = {
          p: "hello world!"
        }
        m.set(o, "content")
        m.get(o) // content
        m.has(o) // true
        m.delete(o)
        m.has(o) // false
    ```

24. 通过传参的方式创建Map实例

    ```javascript
        const map = new Map([
          ["name", "张三"],
          ["title", "Author"]
        ])
    ```

    传参的方式时，参数需满足的条件是：

    + 含有Iterator接口
    + 可以用数组解构的方式，解构为key，value

25. 除了数组，还有哪些类型的数据可以作为Map的参数 

    满足上面两个条件的参数都可以。所以Set实例和Map实例也可以。

    ```javascript
    // 1, Set实例作为参数，创建Map实例
    const set = new Set([
        ["foo", 1],
        ["bar", 2]
    ])
    const m1 = new Map(set)
    // 2, Map实例作为参数，创建Map实例
    const map = new Map([
        ["name", "kk"]
    ])
    const m2 = new Map(map)
    m2.has("name") // true
    ```

26. 通过传参的方式创建Map实例时，执行的过程是？

    创建一个新的Map实例，将传参数组中每个元素按数组解构为key,value,然后用map.set()加到map中。代码演示分析如下：

    ```javascript
        const items = [
          ["name", "张三"],
          ["title", "Author"]
        ]
        const map = new Map()
        items.forEach(([key, value]) => map.set(key, value))
    ```

27. 对同一键值多次赋值时，后面的赋值会覆盖前面的吗？

    会，后面的值会覆盖前面的值。

    ```JavaScript
        const m = new Map([
          ["name", "chenke"]
        ])
        m.set("name", "shenlele")
        m.get("name") // shenlele
    ```

28. 读取一个为未知的值，得到的是什么？

    ```javascript
    const m = new Map([
        ["name", "chenke"]
    ])
    m.get("kkww") // undefined
    ```

29. 键值是对象和数组时，“键”是和什么绑定的？

    和内存地址绑定。同一个地址才是同一个键，否则不同。

30. 如何判断键值相等？

    布尔值true和字符串“true”不是相同键，undefined 和null不是相同的键，NaN是同一个键，+0和-0是同一个键

31. Map实例的属性和操作方法有哪些？

    属性：size， 获取成员个数

    方法：
        `Map.prototype.set(key,value)`   返回实例set
        `Map.prototype.get(key)`   返回key的键值，没找到就返回undefined
        `Map.prototype.has(key)`  找到键值key，返回true；没找到，返回false
        `Map.prototype.delete(key)`  找到键值key，删除，并返回true；  没找到，返回false
        `Map.prototype.clear( )`  清空map实例的所有成员，无返回值

32. Map的遍历方法有哪些？

    `Map.prototype.keys(  )` 键名的遍历器
    `Map.prototype.values(  )`  键值的遍历器
    `Map.prototype.entries(  )`  所有成员的遍历器
    `Map.prototype.forEach(  )`  遍历Map的所有成员

33. Map的遍历顺序是什么?

    插入时候的顺序

34. 遍历器可以用什么获取到？

    用 `for...of`获取到遍历器。下面是用 `for...of`和 `keys()`进行遍历时的用法

    ```javascript
    const m = new Map()
    m.set("name", "kk").set("company", "abc").set("sex", "female")
    for (let key of m.keys()) {
        console.log(key) // name company sex
    }
    ```

35. map实例可以直接遍历吗？相当于 `keys()`, `values()`和 `entries()`中的哪一种？

    map实例可以直接遍历。相当于`entries()`

    ```javascript
    map[`Symbol`.iterator] === map.entries  //true
    ```

36. map可以转化为数组吗？

    可以。因为  `keys()`, `values()`和 `entries()` 返回的都是遍历器。用 `for...of`的时候`m`相当于 `m.entries()`，因此以上4种情况，都能用 `for..of`遍历 。

    扩展运算符可以把遍历器转化为数组。

    ```javascript
    [...m.keys()] // [ 'name', 'company', 'sex' ]
    [...m.values()] // [ 'kk', 'abc', 'female' ]
    [...m.entreis()] // [ [ 'name', 'kk' ], [ 'company', 'abc' ], [ 'sex', 'female' ] ]
    [...m] // [ [ 'name', 'kk' ], [ 'company', 'abc' ], [ 'sex', 'female' ] ]
    ```

37. 对Map实例做filter过滤和map处理，该怎么做？

    filter和map都是数组的方法，所以要先转化为数组

    ```javascript
    	// Map结构的map方法
        let map = new Map([...m].filter(([key, value]) => {
          return key === "company"
        }))
        // Map结构的filter方法
        let map2 = new Map([...m].map(([key, value]) => {
          return [(key + "-cc"), (value + "-kk")]
        }))
    ```

38. map的forEach方法，怎么用？

    ```javascript
    // Map的forEach()方法: 两个参数，一个回调fn( value,key, thisArg)
    // 注意三个参数的顺序。value在key之前，map是函数中this的指向
    // map和set中的forEach方法的表现形式一致
        map.forEach((value, key, map) => {
          console.log(`value: ${value}; key: ${key}; map: ${map}`);
        })
    ```

39. Map转化为数组，数组转化为Map，Map转化为对象，对象转化为Map结构，Map转化为JSON，JSON转化为Map

    ① Map转化为数组: 扩展运算符或者Array.from( )

    ```javascript
    [...m] 
    Array.from( m)
    ```

    ② 数组转化为Map： 二维数组作为 Map构造函数的参数

    ```javascript
    let arr = [[“name", "kk"], ["company, "microsoft"], [""]]
    new Map(arr)  // 得到新Map
    ```

    ③ Map转化为对象：键值都是字符串时，可无损转化为对象

    ```javascript
    function transferToObj(map){
        let obj = Object.create(null)
        map.forEach(([key,value]) => {
            obj[key] = value
        })
        return obj
    }
    ```

    ④ 对象转化为map： 对象的key和value分别对应Map的键-值

    ```javascript
    function objToStrMap(obj){
        let map = new Map()
        Object.keys(obj).forEach(key => {
            map.set(key, obj[key])
        })
        return map
    }
    // 获取到对象的key和value，然后用set方法，往map结构中添加
    ```

    ⑤ Map转化为json：

    ```javascript
    // 键名是字符串: 利用map转化为json
    function transferToJson(map){
        return JSON.stringify(transferToObj(map))
    }
    // 键名不是字符串: 转化为数组json。map转化为数组，然后数组作为JSON.stringify（）的参数
    function transferToJson(map){
        return JSON.stringify([...map])
    }
    ```

    ⑥ json转化为map：

    ```javascript
    // 数组json，数组必须是二维数组
    function jsonToMap(jsonArr){
        let arr = JSON.parse(jsonArr)
        return new Map(arr)	
    }
    // 对象json
    function jsonToMap(jsonObj){
        let obj = JSON.parse(jsonObj)
        return objToStrMap(obj)
    }
    ```

40. WeakMap是什么，有什么作用？

    WeakMap是构造函数，用于生成键-值 对 的集合

41. WeakMap有什么特点？

    三点：

    ① 只接收对象（包含数组）作为键名

    ② WeakMap的键名所指向的对象，不计入垃圾回收机制 【只要所引用的对象的引用都被清除，该对象所占用的内存就会被垃圾回收机制释放】

    ③ 只有get set has delete这四个方法可用。size属性没有。map有的四个遍历方法也没有。

## 解构赋值：

1. 数组解构赋值的匹配模式是怎样的？

   数组匹配模式：左边的变量和右边数组的数据，按顺序匹配

2. 如何完全解构，获取数组的第一个和最后一个元素 [1,2,3,4]?

   完全解构获取第一个和最后一个元素。 let [x21,,,x22] = [1,2,3,4]

(中间不需要的参数，全部空出来)

3. 如何用不完全解构，获取数组的第一个和第二个元素[1,2,3,4]？

   不完全解构。let [x31,x32] = [1,2,3,4]  

4. 数组解构时，默认值是表达式时有什么特点？

   默认值是表达式时，惰性赋值。只有取到默认值时，表达式才会执行。不取默认值时不执行。

5.如何触发解构赋值的默认值（两种方式）？

①    左边的变量，设了默认值，同时，变量在右边没找到

②    左边的变量，设了默认值，同时，变量在右边找到了，但值为undefined

6.对象的解构赋值的内部机制是什么？

①    左边的变量，在右边找相应的key值。如果找到了，将此key的value值，赋给左边对应的变量

7.对象解构赋值，默认值生效的条件是什么？

①    左边的变量在右边找不到 ②在右边找到了，但是是undefined

8.对象解构赋值时，先声明再赋值怎么做？

①    用圆括号，将赋值的语句括起来。如图：

({x4,y4} = {x4:1,y4:21}) 

9.如何将Math.floor和Math.abs解构到floor和abs上？

①    解构赋值的方式，解构到floor和abs上，如图：

let {floor,abs} = Math;

10.如何用对象解构的方式，获取数组【1,2,3,4】的第一位和最后一位数值？

①    数组也可以用对象解构的方式，来进行赋值。如图：

​    let arr1 =[12,1,23,3,45,2,122,56,54];

​    let {0:firstE,[arr1.length-1]:lastE} = arr1;

 

11.字符串为什么可以进行解构赋值？解构一个数组，试着解构下数组的length属性

①    字符串有length属性，能转化为类似数组。

②    let [strA,strB,strC]='helcabc'  

③    解构数组的length属性： let {length:len} = arr1  数组是对象，并且数组对象有length属性

12.试着分析下下面两个函数，当传值不同时，他们分别输出什么？

function **moveFn**({x =0,y=0}={}){

return [x,y];

​    }    

​    function **moveFn2** ({x,y} = {x:0,y:0}){

return [x,y];

}

 

①    两个函数都运用了函数参数赋值和参数的解构赋值。

②    moveFn函数的默认参数是{ }，moveFn2的默认参数是{x:0,y:0}

③    moveFn参数的解构赋值中，x和y的默认值是0，moveFn2中x和y无默认值

13.列举下变量解构赋值的用途。（7种）

①    数据的交换 

let x1 = 12,y1 = 14;

[x1,y1] = [y1,x1];

 

②    函数参数的解构

应用场景：某个obj中有多个属性值，fnPara函数需要对obj的部分值进行操作时，这种方法很方便

function **fnPara**({x,y,z}){

return x*y+z;

​    }

​    let resPara = **fnPara**({x:2,y:3,z:10,m:13})

 

③    函数结果返回，返回多个值

function **returnPara**(pa){

let x = pa+1;

let y = pa*2;

let z = pa*3;

return[x,y,z]

​    }

​    let [first,second,third] = **returnPara**(3);

 

④    提取json值

let datas = {

url:'www.baidu',

time:'2018-8-8 17:59:58',

data:[12,23,34]    }

 

​    let {url:target,time,data:number} = datas;

  target，time和number的值，对应datas的url，time和data

⑤    函数参数的默认值

在参数定义里，定义参数默认值。

⑥    遍历Map结构

map结构的变量可以用for…of进行遍历

​    for(let [key] of map){ 

​    }

​    for(let [,value] of map){

​    }

⑦    输入模块的指定方法：

const { SourceMapConsumer, SourceNode } = **require**("source-map");

 

14.一个变量已经声明过，如何进行结构赋值。分别举个数组和对象的实例

①    声明过的变量，进行数组解构赋值

​    let announce1 ,announce2;

​    [announce1,announce2] = [2,3,4];

 

②    声明过的变量，进行对象解构赋值，加括号

​    let obj1,obj2;

​    ({obj1,obj2} = {obj1:23,obj2:45});



## export和import：

1. css中模块的引入用的什么指令？

   用@import引入。

   ```javascript
   @import  url(global.css)
   @import "global.css"
   ```

   

2. ES6和其它方式的区别？
   ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。而commonJS和AMD模块，都是在运行时确定这些东西。

3. 如何从require来的模块中读取方法？

   let { stat, exists, readFile } = require('fs'); 是整体加载fs模块，生成一个对象，然后再从这三个对象上读取这三个方法 

4. 如何从import来的模块中读取方法？

   import { stat, exists, readFile } from 'fs';   是编译时加载（或静态加载）。表示从fs模块中只加载这三个方法，其他不加载。 由于只加载部分，ES6的import加载无法引用模块本身，因为他并不是对象。

5. ES6中需要指定严格模式（static）吗？

   不需要。

6. 严格模式下有什么特点？

   顶层的this指向的是undefined。不能在顶层代码中使用this。

7. export命令的作用是什么？

   提供模块的对外接口。

8. export能获取模块内部的实时值吗？为什么?

   能获取模块内部的实时值。因为export输出的接口和它的值是一一对应的关系。可以取到模块内部实时值。

9. export可以用在模块中间吗？

   export可以用在模块任何位置，只要是顶层。但是不能放在块级作用域中，会报错。

10. export如何输出单个变量（或方法，或类）？

    常用的有三种输入方式

    ```javascript
    // 变量的定义和输出放在一起
    export var m = 1
    // 普通方法定义一个变量，然后花括号输出
    var m = 1
    export {m}
    // 定义一个变量，重命名的方式输出
    var m = 1
    export { m as n}
    ```

    

11. export批量如何输出？

    ```javascript
    var name = "kk"
    var height = "172cm"
    // 尾部批量输出
    export {name, height as h}
    ```

    

12. import 有提升效果吗？

    有的。看例子

    ```javascript
    foo()
    import { foo } from "myModule"
    ```

    以上不会报错，因为引入的那一行，被提升到模块的头部了

13. import关键字后面能接表达式或计算吗？

    不可以。因为表达式和计算是只有在运行时才能得到结果的用法。

14. 当同时从一个module输入多次时，得到的是什么结果？

    import同一个模块时，会合并import的内容。看例子

    ```javascript
    // 例子一：
    import {foo} from "myModule"
    import {bar} from "myModule"
    // 不会报错，而且等价于下面
    import {foo,bar} from "myModule"
    // 例子二:重复执行多次所加载的模块时，只会执行一次
    import "loadsh"
    import "loadsh"
    ```

15. 如何实现模块的整体加载？

    circle.js中export了两个function，用下面的语句，实现整体加载这两个方法，并重命名为circle。

    ```javascript
    import * as circle from "./circle.js" 
    ```

    输入的是circle.js中输出的非默认接口

16. 能不能改变import进来的变量的值？

    不可以。如果是简单类型的变量，不能直接改。如果是复杂类型的变量（对象或者数组），可以修改属性的值。

    但是并不建议这么改，会让代码不好调试。

    ```javascript
    import * as circle from "./circle.js"
    circle.foo = "hello"
    circle.area = function(){ }
    ```

    这样会报错

17. from后面接的什么？

    绝对路径或相对路径。如果是模块名，就要自己做一下配置。文件的.js后缀可以省略。

18. export default的使用场景

    export时，需要对输出的变量命名。export default 的意思是，当前输出的变量命名为default

    ```javascript
    // circle.js中
    export default fuction circle(){}  // circle可要可不要
    // 输入时，为默认输出的内容命名
    import area from "./circle"
    ```

    

19. 一个模块中，可以使用多次export default输出多个默认变量吗？

    不可以。export default是模块的默认输出，每个模块只能有1个。

20. 如何输入模块的默认变量或方法？

    `import ladash from  'lodash'`

21. 如何输入模块的默认方法和其它接口

    `import name, { each, forEach} from "loadsh"`

22. export default 输出一个类

    ```javascript
    export default Point{
        
    }
    ```

23. export 和import的复合写法

    ```javascript
    export { foo, bar } from "my_module"
    ```

    合并为一行，只是将 foo和bar这两个接口转发出去。当前模块下并不能用foo和bar变量

24. 具名接口改为默认接口和默认接口改为具名接口

    ```javascript
    export { es6 from default } from "./someModule"
    export { default from es6 } from "./someModule"
    ```

25. 如何仅仅输出模块的非默认接口

    `export * from "./someModule"`

26. 模块之间可以继承吗？

    可以实现继承 。看下面的例子：

    ```javascript
    // circleplus.js中
    export {area as circleArea} from "./circle"
    export var e = 2.718
    export default function(x){
        return Math.exp(x)
    }
    //main.js中
    import * as math from "circleplus"
    import exp from "circleplus"
    console.log(exp(math.e))
    ```

    分析：

    `circleplue.js`中，输出了两个具名接口，`circleArea`和 `e`，一个默认接口

    `main.js`输入了 `circleplus.js` 具名接口和默认接口欧。具名接口是被整体加载，默认接口重命名为exp

27. 常量可以跨模块共享吗？

    可以的。

    ```javascript
    // constant.js
    export const A = 1
    export const B = 2
    // test1.js
    import * as consts from "/constant.js"
    console.log(consts.A)
    console.log(consts.B)
    ```

28. `import()`为什么会出现？

    首先这里的 `import()`后面没接 `from`，是个方法。import和export命令能在模块顶层使用，不能写在代码块和函数里。当需要在运行时加载模块就不能加载实现。

29. `import()`和Node的 `require`方法有什么区别？

    `import()`是异步加载，`require`方法是同步加载

30. `import()`使用的场合有哪些？

    1. 按需加载： 例如写在事件的监听函数中

       ```javascript
       buttn.addEventListener("click", event => {
       	import("./dialogBox.js")
       })
       ```

       

    2. 条件加载：

       ```javascript
       if(condition){
           import("moduleA").then()
       }else{
           import("moduleB").then()
       }
       ```

    3. 动态的模块路径：

       ```javascript
       import(f()).then(...)
       ```

31. `import("./myModule.js")`加载模块成功后，返回值是什么？如何获取？

    返回值是输出接口组成的对象。通过 `.then()`方法获取。

    比如 `myModule.js`输出了两个接口 `export1`和 `export2`。用 `import()` 方法加载之后，结合对象的解构赋值，处理过程如下。

    ```javascript
    import("./myModule.js").then(({export1, export2}) => {
        // 逻辑处理
    }).catch(() => {})
    ```

    如果 `myModule.js`只输出了一个 `default`, 那么then的参数就是`default`指代的变量（或方法或函数）。

32. `import()`同时加载多个模块时，怎么用？结合 `Promise.all()`和 `async`一起说明。

    结合`Promise.all()`一起用

    ```javascript
    Promise.all([
        import("./myModule1.js"),
        import("./myModule2.js"),
        import("./myMudole3.js")
    ]).then(([module1, module2, module3]) => {
        ...
    })
    ```

    结合`async`一起用

    ```javascript
    async function main(){
        const myModule = await import("./myModule.js")
        const { export1, export2 } = await import("./myModule.js")
        const [module1, module2, module3] = 
              await Promise.all([
                  import("./module1.js"),
                  import("./module2.js"),
                  import("./module3.js")
              ])
    }
    main()
    ```

## Promise对象

2. Promise是什么？我们说的promise又是什么？Promise传什么参数?

   Promise是构造函数，因为构造函数通过new关键字，得到的是对象。因此，通过下列赋值，`let promise = new Promise( /* some params */)`，得到的promise是对象。

   `Promise`接收函数作为构造函数的参数，函数包含 `resolve`和`reject`参数，他们是两个函数，作用看例子。

   ```javascript
   let promise = new Promise(function(resolve, reject){
       if(/* 异步调用成功*/){
          // 将promise对象的状态从pending变为resolved，并将处理的结果传出去
          resolve(value)
        }else{
          // 将promise对象的状态从pending变为rejected，并将处理的结果传出去
          reject(error)
        }       
   })
   ```

   

3. promise对象有哪几种状态？状态是怎么变化的？

   `pending`， `fulfilled` ， `rejected`

   状态只能从 `pending`变为 `fulfilled` 或者 `pending`变为 `rejected`
   
3. promise对象有什么用？

   两个角度：

   + 作用： 里面保存着异步操作的状态(pending, resolved, rejected)，并且状态锁定了（变为 `resolved`或者 `rejected`）之后就不能更改
   + 语法：一个对象，从它可以获取异步操作的消息

4. promise对象中的（异步操作状态）怎么获取？

   通过 `then`方法获取。`then`方法接收两个函数作为参数。第一个参数是`promise`对象变为`resolved`状态时的回调函数，必填。第二个参数是`promise`变为`rejected`状态时的回调函数，非必填。看例子

   ```javascript
   function timeout(ms){
       return new Promise((resolve, reject) => {
           setTimeout (resolve, ms, "hahaha")
       })
   }
   timeout(20000).then((value) => {
       console.log(value)
   })
   ```

   上面的执行结果是 20s之后输出 hahaha

5. 下面`Promise`实例执行的结果

   ```javascript
   setTimeout(() => {
     console.log("timeout!!")
   }, 0);
   
   let promise = new Promise(function(resolve, reject){
     console.log("Promise")
     resolve()
   })
   
   promise.then(function(){
     console.log("resolved.")
   })
   
   console.log("Hi")
   ```

   依次输出：  Promise  Hi  resolved  timeout!!

   因为promise实例是在创建之后就马上执行。promise的then方法是当前脚本的所有同步任务执行完了之后，也就是`本轮事件循环`结束时执行。setTimeout是`下一轮事件循环`开始时执行。

6. 用promise封装一个ajax请求的例子

   ![promise封装的ajax](D:\vscodeProjects\es6-ryf\images\用promise写一个ajax请求.png)

7. 如果promise2，resolve的是另一个promise1，那么promise2的自身的状态失效，promise2的状态由promise1的状态决定。看例子：

   ![p1决定p2的状态](D:\vscodeProjects\es6-ryf\images\promise2的状态由promise1决定.png)

8. `resolve`或者`reject`之后，`function(resolve, reject){ }`函数（构造函数 `Promise`的传参）还会继续往下执行吗？ 

   会的。因此 `resolve`或者`reject`的值直接`return`出去。不要在后面继续写逻辑了。后续的逻辑，应该写在实例的 `then`方法中。

9. promise实例是有哪些原型方法？

   看图，主要有`catch,then,finally,constructor`

   ![](D:\vscodeProjects\es6-ryf\images\promise实例打印出来的内容.png)

10. `Promise.prototype.then`方法怎么用？

    为实例添加状态改变时的回调函数。包含两个参数，都是函数。第一个函数是实例resolved状态时的回调，函数的参数是resolved时传出的值。第二个函数是实例rejected状态时的回调，参数是rejected时传出的值。

    then方法返回的是一个新的Promise实例，前面的then的处理结果，可以作为后一个Promise实例的参数。看例子

    ```javascript
    getJSON("./posts.json").then(function(json){
        return json.post
    }).then((post) => { }) // josn.post作为参数给此处的then方法的第一个函数。
    ```

    

11. `Promise.prototype.catch`方法怎么用？如何理解？

    + rejected时，等同于抛出错误。
    + catch会捕获rejected时或者then在运行时抛出的错误。
    + promise对象的错误有冒泡性质，会一直向后传递，会被最后一个catch语句捕获。
    + 不要在then方法里面定义rejected 状态的回调函数，总用链式的方法使用catch方法，可以捕获所有的rejected和then方法在处理数据时抛出的错误。
    +  catch方法还是返回一个promise对象。

12. promise对象抛出的错误会传递到外层代码吗？

    不会，即使抛出了错误，promise外部的代码也会继续执行。

13. `Promise.prototype.finally()`如何用？

    + `finally`方法用于指定不管promise实例最后状态如何，都会执行的操作。
    + `finally`方法的回调函数没有任何参数。`finally`方法里的操作，和promise的状态无关，不依赖promise的执行结果。

14. `Promise.all()`方法的作用？

    将多个promise实例，包装成一个新的promise实例。

    ```javascript
    const p = Promise.all([p1, p2, p3])
    ```

    注意的点：

    + 参数是有Iterator数据结构的参数。比如数组。

    + p1p2p3全部是fulfilled时，p才为fulfilled，此时p1p2p3的返回值组成数组，给p的then方法

    + p1p2p3中有一个被rejected时，p为rejected，第一个rejected的实例的返回值，传给p的catch方法

    + p1p1p3是Promise.all接收到的参数，如果参数不是promise，则会调用 `promise.resolve()`方法，转化为promise实例，再做进一步处理。这一点要特别注意，看下面的例子:

      ```javascript
      const p1 = new Promise((resolve, reject) => {
        resolve('hello');
      })
      .then(result => {
        return result + "_chenke"
      });
      
      const p2 = new Promise((resolve, reject) => {
        throw new Error('报错了');
      })
      .then(result => result)
      .catch(e => console.log(e));
      
      Promise.all([p1, p2])
      .then(result => {
        console.log("resolved 了")
        console.log(result)
      })
      .catch(e => {
        console.log("reject 了")
        console.log(e)
      });
      ```

      上面例子中，p1是then方法返回的promise的状态，是 `Promise.resolve("result + "_chenke"")`, p2是catch方法返回的promise的状态，是`Promise.resolved(unefined)`。所以，Promise.all的状态是resolved。执行的是它自己的then方法。以上的自行结果是。

      ![](D:\vscodeProjects\es6-ryf\images\Promise.all.png)

      以上的 `promise.all`如果希望它的状态是rejected，可以把p2的catch删掉。此时p2是新创建的实例的promise，状态经过 `throw new Error('报错了')`，变为rejected

      ```javascript
      const p2 = new Promise((resolve, reject) => {
        throw new Error('报错了');
      })
      .then(result => result)
      ```

15. `Promise.race()`方法的作用？

    将多个Promise实例，包装成一个新的Promise实例。

    ```javascript
    const p = Promise.race([p1, p2, p3])
    ```

    注意点：

    + p1p2p3中主要有一个实例先改变了状态，p的状态也随之改变。

    + p1p1p3是Promise.all接收到的参数，如果参数不是promise，则会调用 `promise.resolve()`方法，转化为promise实例，再做进一步处理。

    + 应用实例： 某个请求，设定一个默认时间，如果5秒没返回，则promise状态变为resolved

      ```javascript
      const p = Promise.race([
        fetch('/resource-that-may-take-a-while'),
        new Promise(function (resolve, reject) {
          setTimeout(() => reject(new Error('request timeout')), 5000)
        })
      ]);
      
      p.then(console.log).catch(console.error);
      ```

16. `Promise.allSettled()`的语法

    接收的参数： 一组promise实例，如p1,p2,p3

    执行条件：所有promise状态定型了之后

    返回值： 新的promise实例

    返回值说明：

    + 新的promise状态只能是fulfilled，不能是rejected
    + 由于上面原因，用then方法，可以把返回的结果，在回调函数中进行处理。
    + 回调函数的参数是一个数组，数组的每个成员是对象，和p1p2p3的结束状态相关
    + 对象是 `{ status: "fulfilled", value: 42}` （fulfilled的promise）或者 `{status: "rejected", reason: 8}`(rejected的promise) 

17. `Promise.allSettled( )`使用场景是什么？

    也是接收一组promise实例。但是要等到所有的实例都有结束（包括resolved和rejected）时，allSettled才会结束。

    使用场景： 不关心异步操作的结果，只关心操作有没有全部结束。`Promise.all()`无法确保所有的异步操作都结束。

    ```javascript
    const resolved = Promise.resolve(42);
    const rejected = Promise.reject(-1);
    
    const allSettledPromise = Promise.allSettled([resolved, rejected]);
    
    allSettledPromise.then(function (results) {
      console.log(results);
    });
    // [
    //    { status: 'fulfilled', value: 42 },
    //    { status: 'rejected', reason: -1 }
    // ]
    ```

18. `Promise.any()`的语法

    接收的参数： 一组promise实例，如p1,p2,p3

    执行条件：三个传入的参数实例，有一个变为fulfilled，则`Promise.any( )`是fulfilled状态。三个都变成rejected，则`Promise.any( )`是rejected状态

    返回值： 新的promise实例

    最新版的chrome中，`Promise.any()`并不能用。因此， 看官网例子

    ```JavaScript
    var resolved = Promise.resolve(42);
    var rejected = Promise.reject(-1);
    var alsoRejected = Promise.reject(Infinity);
    
    Promise.any([resolved, rejected, alsoRejected]).then(function (result) {
      console.log(result); // 42
    });
    
    Promise.any([rejected, alsoRejected]).catch(function (results) {
      console.log(results); // [-1, Infinity]
    });
    ```

19. `Promise.resolve()`的语法？

    `Promise.resolve(param)`是将传入的param转化为promise对象。等价写法是：

    ```javascript
    Promise.resolve("foo")
    // 等价于
    new Promise((resolve) => resolve("foo"))
    ```

    `Promise.resolve(param)`的参数 `param`有下面四种情况：

    + 一个Promise实例： 原封不动返回实例

    + `thenable`对象（具有then方法的对象）：先将对象转化为promise对象，然后立即执行thenable的then方法。看例子：

      ```javascript
      let obj = {
        then: function(resolve, reject) {
          resolve(42);
        }
      };
      
      let p1 = Promise.resolve(obj);
      p1.then(function(value) {
        console.log(value);  // 42
      });
      ```

      `Promise.resolve(obj)`执行时，是先将obj转化为promise，然后马上执行 `obj.then()`。

    + 不是promise实例，不是包含 `then`的对象： 返回一个状态是resolved的promise，并且传出来的值是这个数本身。看例子：

      ```javascript
      const p = Promise.resolve({ name: "cc", age: 32})
      p.then( val => {console.log(val)}) // {name: "cc", age: 32}
      ```

    + 不传任何参数: 直接返回一个 resolved状态的promise对象。通常用于需要直接得到一个promise对象的时候。注意，立即resolve( )得到的promise对象，在本轮“事件循环”的结束执行。看例子：

      ```javascript
      setTimeout(() => {
          console.log("three")
      } )
      Promise.resolve().then(() => {
          console.log("two")
      })
      console.log("one")  
      ```

      依次输出 one, two ,three

      setTimeout是下轮事件开始执行。Promise.resolve()是本轮事件结束之后执行.

20. `Promise.reject()`方法的语法？

    返回一个`rejected`状态的promise对象。但是它和 `Promise.resolve(param)`不同。接收的参数会原封不动编程后续方法的参数，不管这个参数是什么。看例子：

    ```javascript
    const thenable = {
        then(resolve, reject){
            reject("出错啦")
        }
    }
    Promise.reject(thenable).catch(e => {
        console.log(e === thenable)  // true
    })
    ```

    

21. `Promise.try()`提案的背景

    不知道或不想区分，函数f是同步函数或是异步函数，但是想用Promise来处理它，都用then方法指定下一步流程，用catch处理f抛出的错误。

22. 如何让同步函数同步执行，异步函数异步执行，并且有统一的api呢？

    + `async`函数来写

      ```javascript
      const f = () => console.log('now');
      (async () => f())();
      console.log('next');
      // now
      // next
      ```

    + `new Promise()`来写

      ```javascript
      const f = () => console.log('now');
      (
        () => new Promise(
          resolve => resolve(f())
        )
      )();
      console.log('next');
      // now
      // next
      ```

23. `Promise.try()`的例子

    ```javascript
    function getUserName(){
      return Promise.try(() => {
        return database.user.get({id: userId});
      }).then(user => {
        return getMetadataFor(user);
      }).then(userMetadata => {
        return userMetadata.name;
      }).catch((e) =>{
        console.log(e)
      })
    }
    ```

    + 上面的例子是用 `Promise.try()`对函数做一个封装，catch中可以捕获到 `Promise.try()`中函数的所有同步和异步的异常。
    + `Promise.try()`像 `.then`一样，但是前面不需要promise对象。

25. `Promise.try()`的用法

## Iterator和`for...of`循环

1. Iterator是什么？

   + Iterator（遍历器）是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。
   + 任何数据结构只要部署了Iterator接口，就能完成遍历操作。

2. Iterator接口的作用?

   + 为各种数据结构提供统一、简便的访问接口
   + 使数据结构的成员能按某种次序排列
   + `for...of`作为新的遍历命令可以给 `Iterator`接口使用

3. `Iterator`的遍历过程是什么？

   + 创建一个指针对象（遍历对象），指向数据结构的起始位置
   + 调用指针对象的 `next`方法，使指针指向数据结构第一个成员
   + 调用指针 `next`方法，使指针指向第二个成员
   + 重复step2和step3，使指针对象指向数据结构的结束位置

   说明：

   + 每一次调用next方法，都会返回数据结构的当前成员信息，一个对象。这个对象包含两个属性，value和done。value是当前成员的值，done是个布尔值，表示遍历是否结束。

4. 默认的Iterator接口部署在哪里？

   数据结构的 `Symbol`.iterator 属性上。

5. 如何判断一个数据结构是否可遍历？

   两种方法：

   + 一种数据结构只要部署了Iterator接口，就称为这种数据结构可遍历。
   + 一个数据结构只要有`Symbol`.iterator属性，，它也是可遍历的。

6. 原生具备Iterator接口的数据结构如下？

   + Array
   +  Map
   + Set
   + String
   + TypedArray
   + 函数的Arguments对象
   + Generator对象
   + NodeList对象

7. 给对象定义一个``Symbol`.iterable`属性后，对象就是可遍历的。看例子：

   例子1：

   ```javascript
   const obj = {
       [`Symbol`.iterator]: function(){
           return {
               next: function(){
                   return {
                       value: 1,
                       done: true
                   }
               }
           }
       }
   }
   ```

   例子2：

   ```javascript
   class RangeIterator {
     constructor(start, stop) {
       this.value = start;
       this.stop = stop;
     }
   
     [`Symbol`.iterator]() { return this; }
   
     next() {
       var value = this.value;
       if (value < this.stop) {
         this.value++;
         return {done: false, value: value};
       }
       return {done: true, value: undefined};
     }
   }
   let r = new RangeIterator(2,6)
   // 新创建的实例可遍历
   for(let i of r){
     console.log(i)
   }
   ```

   

8. ``Symbol`.iterator`方法返回的是什么？

   返回的是遍历器生成函数。

   这个函数执行结果是，指针对象。有next方法。

   调用next方法后，返回的是一个对象。

   对象包含value和done属性

9. 有遍历器的数据结构除了用for...of，还能有别的什么方法遍历？

   用 `while`方法循环。看例子：

   ```javascript
   let iterator = this.arr[`Symbol`.iterator]()
   // iterator是一个遍历器对象，有next方法，没执行一次next方法，都会得到一个{value,done}对象。遍历一次一共会调用len +1次，最后一次的结果是 {value: undefined, done:true}
   let result = iterator.next()
   while(!result.done){
       let x = result.value
       console.log(x)
       result = iterator.next()
   }
   ```

10. 调用`Iterator`接口的场合

    + 解构赋值：数组和 `set`做解构赋值时，会自动调用 ``Symbol`.iterator`方法

      ```
      let set = new Set().add("a").add("b")
      let [x, y] = set
      ```

    + 扩展运算符：部署了Iterator接口的数据结构，都可以用扩展运算符，将其转化为数组

      ```javascript
      let arr = [...iterable]
      ```

    + yield*: `yield *`后面跟的是可遍历的解构，会自动调用 `Iterator`接口

    + 其他场合：

      由于数组的遍历调会调用遍历器接口，所以任何接受数组作为参数的场合，都调用了一次遍历器接口  `Array.from()` `for...of` `Map() set() WeakMap() WeakSet() Promise.all() Promise.race() `等。

11. 一个数组有数字索引和非数组索引，用`for...of`和 `for...in`分别返回什么？

    非数字索引没有算在数组的length属性中。

    `for...of`只返回数字索引，而 `for...in`返回所有数字和非数字索引

12. 用 `for...of`遍历Set和Map结构是，返回的分别是什么？返回的顺序呢？

    Set结构返回的是一个值，Map返回的是一个`[key, value]`数组

    返回的顺序是各个成员被添加进数据结构的顺序。

13. 对象可以直接用 `for...of`循环吗？

    不可以。对象没有部署 `iterator` 接口，直接用 `for...of`会报错。

    曲线救国的方法是，用 `Object.keys()`遍历对象的属性，得到一个数组，然后遍历这个数组。

14. forEach遍历数组有什么缺点？

    无法中途跳出 `forEach`循环。用`break`或者`return`都不行

15. `forEach`, `for...in` 和`for...of`在遍历数组的时候，有什么区别？

    `forEach`无法在中间终结，必须遍历完，只能终结当前执行的语句。

    不能使用`break`,也不能使用`continue`。如果希望终结当前语句，用`return`

    ```javascript
    arr.forEach(item => {
        console.log("当前输出的是：  ", item)
        if(item % 2){ return }
        console.log(item)
    })
    ```

    `for...in`支持 break，continue，return，上面三种形式的表现都非常正常，但是也有缺点，缺点有：

    + 遍历时，得到的键名是字符串“0”，“1”，而数组下标是数字，0，1

    + 不仅遍历数字键名，也遍历其他键名（手动添加的键名，或prototype对象上的）

    + 有些情况下，`for...in`会用任意顺序遍历键名

      `for...in`是为遍历对象设计的，不适合数组

      ```javascript
      for(let i in arr){
          if(!(arr[i] % 7)){
              return
          }
          console.log(arr[i])
      }
      ```

    `for...of`优点：

    + 可以提前结束`所有`循环，支持`break`

    + 可以提前结束`本次`循环，支持 `continue` 和`return`，用`continue`和 `return`的计算结果相同

      ```javascript
      for(let i of arr){
          if(!(i % 7)){
              continue
          }
          console.log(i)
      }
      ```

## Generator函数语法

1. 基本概念
   + 异步编程解决方案
   + 是一个状态机，封装了多个内部状态
   +  `Generator`函数的返回值是一个遍历器对象，此对象可以依次遍历 `Generator`函数内部的每一个状态
   + `Generator`函数 = 状态机 + 遍历器对象
   + `Generator`函数有两个特点， `function* HelloGenerator(){}`和 `yield`关键字
   
2. `Generator`函数如何调用?返回值是什么？

   函数名后加一对圆括号调用。但是调用后，该函数并不执行，返回值也不是函数运行的结果，而是一个指向内部状态的指针对象（Iterator Object）。

3. `Generator`函数执行的过程？

   括号调用后返回指针对象。调用此对象的 `next`方法，指针对象就从函数头部或者上次停下来的地方开始执行，直到遇到 `yield`或者 `return`.因此 `Generator`函数是分段执行，`yield`是暂停标记，`next`是继续执行的方法。

4. 每次调用 `next()`方法时，返回的对象有哪些属性，属性的值是多少？

   返回的对象有 `value`和 `done`属性。value是当前 `yield`表达式后面的表达式的 值，或者 `return` 后的表达式的值，如果是函数尾部，`value`就是 `undefined`。`done`是布尔值，表示遍历是否结束。

   当Generator函数运行完毕时，返回的对象是 `{value: undefined, done: true}`。以后再调用，返回的都是这个值。

5. `yield`表达式后面的表达式，什么时候执行？

   调用了 `next`方法，并且指针指向该语句时，才会执行

6. `yield`表达式和 `return`语句的异同是什么？

   相同点： 

   + 都可以返回紧跟在后面的表达式的值

   不同点：

   + 每次遇到 `yield`，会 `暂停`执行，等下次调用 `next`方法，再从暂停的位置，继续向下执行
   + 遇到return后，程序就执行完了。即此时返回的对象中，`done`属性是 `true`
   + 每个函数只能有一个 `return`语句，但是可以有任意多个 `yield`表达式

7. `Generator`函数可以不用 `yield`表达式吗？

   可以。如果不用的话。就是最普通的暂缓执行的函数。看下面的例子

   ```javascript
   //定义一个不含generator的函数
   function* a(){
       console.log("执行了~")
   }
   // 用括号调用时，函数并没有真的执行
   let obj = a()
   // 调用obj的next()方法时，才是真的执行
   setTimeout(() => {
       obj.next()
   }, 2000);
   // 执行结果是，2s后，才打印 执行了~
   ```

8. `yield`可以用在普通函数里吗？

   不可以，会报错。

   注意很隐蔽的地方，比如 `forEach`的回调函数参数。

9. `yield`表达式如果要用在另一个表达式中，怎么用？

   `yield`的内容必须放在 `圆括号`中

   ```javascript
   // error
   console.log("hello" + yield 123)
   // correct
   console.log("hello"+ (yield 123))
   ```

10. `yield`表达式用作函数参数或赋值表达式右边，怎么用？

    可以不用括号。

    ```javascript
    function* demo(){
        foo(yield 'a', yield 'b')
        let input = yield
    }
    ```

11. `Generator`函数和`Iterator`接口有什么关系？

    任何一个对象 `obj` 的 ``Symbol`.iterator`方法（**方法也叫函数**），都是它的遍历器对象生成函数。`obj[`Symbol`,iterator]()`的结果是一个遍历器对象。

    `Generator`函数是 **遍历器生成函数**。因此 `Generator`和 ``Symbol`.iterator`是同种作用的函数。因此 ，可以将`Generator`函数赋值给 ``Symbol`.iterator`方法。

12. `Generator`函数执行后生成遍历器对象，这个遍历器对象`g`和 ``Symbol`.iterator`有什么关系？

    ```javascript
    g[`Symbol`.iterator]() === g
    ```

13. 如何干预 `Generator`函数的执行过程？

    可以在`next()`方法中传入参数。干预的原理是：**上一个**yield语句的返回结果是遍历器对象next方法的传参，当next不传参数时，上一个yield语句返回的是undefined；当next语句传了参数时，上一个yield语句返回的就是这个参数。可以看个例子

    ```JavaScript
     generator4(){
         let _this = this
         function* f(){
             for(var i = 1; true; i++){
                 _this.count = i
                 console.log("before")
                 var reset = yield i;
                 console.log(reset)
                 if(reset){ i = 0  } 
             }
         }
         // 第一次进来的时候，先获取遍历器对象
         if(!this.g4){
             this.g4 = f()
         }
         // 上一步不是7的倍数时,不干预；是7的倍数时，传true
         let valueObj = this.count % 7 ? this.g4.next(false) : this.g4.next(true)
         console.log(valueObj)
         // 打印的内容从 {value: 1, done: false} 到 {value: 7, done: false}
     },
    ```

    给按钮绑定了click事件，事件处理函数是 `generator4`。本来f是一个可以无限执行的Generator函数，由于next传参的干预。不停的在1到7之间循环。

    第二次整理:  

    点第一下时，走到第一个yield。打印了 before，然后打印valueObj（遍历器的next( )方法的返回值）。

    点第二下时，`reset是上一个yield表达式的返回值`，是false。打印false，然后before，然后是valueObj

    点第三下到第七下，打印的内容都和第二下相同

    由于点第7下时，执行的是this.g4.next(true)，所以第8下，`reset的值就是true`，打印true。所以循环结束时，i的值是0。继续往下，打印before，然后是valueObj {value: 1, done: false}

    第三次整理：

    【每走一步，会得到一个valueObj,并暂停在当前的yield表达式所在行】

14. 第二个用next传参的例子：

    ```javascript
    generator5(){
        function* foo(x){
            var y = 2 * (yield (x + 1))
            var z = yield(y / 3)
            return !(x + y + z) ? x : x + y + z
        }
        if(!this.g5){
            this.g5 = foo(5)
        }
        console.log(this.g5.next())
    },
    ```

    分析：

    foo函数一共有两个yield端点，一个return结束点。

    在第一个断点时，`next()`的执行结果是yield后面表达式的值，因此是`{value: 6, done: false}`。

    第二个断点时，因为此时next()没有传参数，因此第一个yield表达式的返回结果是undefined。`next()`的执行结果是第二个yield后面表达式的值，因为y是 `undefined`，所以 `y/3=NaN`。因此返回结果是 `{value: NaN, done: false}`

    第三次next时，因为此时next()没有传参数，因此第二个yield表达式的返回结果是undefined。z就是NaN。第三次next的执行结果就是return的值， `x + y + z`结果是 `NaN`，总的返回结果是 `{value: 5, done: true}`

    以后每次next，返回的都是Generator函数运行完毕的结果 `{value: undefined, done: true}`

    当执行的是 `this.g5.next(6)`时，value的值依次是： 6,4,23  xyz依次是5,12,6

    从上面可以按到，Generator函数内部的变量，在运行过程中，都会暂存在内部。

15. 再看一个例子：

    ```javascript
    function* foo(x){
        var y = 2 * (yield (x + 1))
        var z = yield(y / 3)
        return x + y + z
    }
    var b = foo(5)
    console.log(b.next())
    console.log(b.next(12))
    console.log(b.next(13))
    ```

    输出结果是 `{value: 6, done: false}` `{value: 8, done: false}` `{value: 42, done: true}`。

    第一句执行结果好说。第二句时，y是24，返回对象的value是24/3。第三句时，z是13，返回对象的value是5+24+13=42

16. 第一次使用next方法，传参有效吗？

    无效。因为next方法的参数是上一个yield表达式的返回值。

17. 使用for...of循环遍历Generator函数生成的Iterator对象，输出什么？

    ```javascript
    function* foo(){
            yield 1;
            yield 2;
            yield 3;
            yield 4;
            yield 5;
            return 6;
          }
          for(let v of foo()){
            console.log(v)
          }
          /* 依次打印出 12345 */
    ```

    输出yield后面的值。遇到done是true的时候就终止，并且不会打印出返回的对象（return 后面的值），即return返回的值不会被打印出来。

18. 使用 for...of的时候，需要用next方法吗？

    不需要。可以直接获取到yield后面的值。

19. 对象能不能用for..of 进行遍历？

    对象没有遍历接口，无法用 for...of循环

20. 如果要让对象用 for...of接口，怎么做？

    + 通过Generator函数，加上这个接口

      for...of  遍历Generator函数，返回的是yield后面的值

    + 将Generator函数加到对象的`Symbol`.iterator属性上

      ```javascript
      obj[`Symbol`.iterator] = generatorFn;
      ```

      

21. 除了for...of，还有哪些可以用遍历器接口？

    Array.from（），扩展运算符（...），解构赋值

    比如：

    numbers 是一个Generator函数，则可以将numbers返回的Iterator对象作为参数，做以下处理：

    ```javascript
    [...numbers()]
    Array.from(numbers())
    let [x,y] = numbers()
    for(let n of numbers()){
        console.log(n)
    }
    ```

    **四种遍历 Iterator对象的方式**，后面还有`yield*`,也可以遍历Iterator接口。

22. General.prototype.throw() 是什么？

    Generator函数返回的遍历器对象，都有throw方法，可以在函数体外抛出错误，然后在Generator函数体内捕获。

23. Generator函数体内部没有部署 try...catch...，那么throw抛出的错误，去哪里了？

    + throw抛出的错误，会被外部的try...catch... 方法catch住
    + 如果内部和外部都没有catch，程序将直接报错，中断执行

24. Iterator对象的throw方法，可以第一次被内部catch住吗？

    throw方法抛出的错误要被内部捕获，前提是必须至少执行过一次next方法。否则会报错。

25. Generator函数内部部署了 try...catch... 块，遍历器的throw方法抛出的错误，会影响下一次遍历吗？

    不会，不影响下一次遍历。每次throw方法被捕获后，会附带执行一次遍历器的next方法。

26. Generator函数体外抛出的错误，可以被函数体内捕获；Generator函数体内抛出的错误，可以被函数体外的catch捕获

27. Generator执行过程中抛出错误，且没有被内部捕获，就不会执行下去。如果继续调用next，则指针对象对Generator函数的遍历将终结，返回 `{value: undefined, done: true}`

28. 遍历器对象的return方法，有什么用？

    + 终结遍历Generator函数，继续next后，返回 ` value: undefined, done: true `
    + 返回一个对象，value是return后给定的值  ` {value: "foo", done: true} `

29. 遍历器对象的return方法和try..finally一起的时候，执行顺序是什么？

    + 当遍历器的return方法写在try中时，return后面的try代码将不会执行，直接跳到finally，执行finally部分的代码，finally的代码执行完后，再回头去执行return
    + return方法出现子finally中，finally会继续执行。只有在finally执行完后，继续调用next方法，return代码才会继续执行。继续next，将终结遍历Generator函数。

30. `next()`, `throw()`, `return()`的共同点是什么？

    让Generator函数回复执行，并且用不同的语句替换yield表达式

    `next(param)`相当于将上一个yield表达式替换为值param。如果param不传，相当于替换为undefined。

    

31. yield* 表达式出现的背景是什么？

    Generator函数foo和bar，如果要在bar部调用foo，则需要在bar内部，手动完成对foo的遍历

    ```javascript
    function* foo() {
      yield 'a';
      yield 'b';
    }
    function* bar(){
        yield "x";
        yield* foo()
        yield "y"
    }
    ```

    由上可知，`yeld*`后面跟的是 `Generator`表达式的执行结果。

    上面表示， `yield* foo()`  相当于  `yield "a"; yield "b"`

32. yield* 表达式的语法意义是什么？

    如果yield表达式后边跟的是一个遍历器对象，则需要在yield后面跟一个星号 `*` 

33. 两个Generator函数A，B，在A里面使用yield* B，是什么意思？

    分两种情况

    + B里面没有return：相当于在A里面部署了一个 `for...of`循环

      ```javascript
      yield* B
      // 相当于
      for(var value of B){
      	yield value
      }
      ```

      

    + B里面有return：此时，return后面的结果不会被遍历到，需要用 `var value = yield* iterator`的形式获取return语句的值。

      ```javascript
      function* A(){
          yield "hello";
          let c = yield* B();
          console.log(c)
          yield "chenke!"
      
      }
      function* B(){
          yield "how";
          yield "are";
          return "you"
      }
      let arr = [...A()]
      console.log(arr)
      ```

      上面代码的运行结果是，先打印出 `you`，然后打印数组 `["hello", "hao", "are", "chenke!"]`

34. `yield*`后面跟的是  Iterator 接口 时，处理结果会如何？比如数组

    ```javascript
    function* A(){
        yield* ["a","b","c"]
    }
    let i = 0
    let obj = A()
    while(i < 4){
        console.log(obj.next())
        i++
    }
    ```

    跟数组时，`yield* ["a","b","c"]`返回的是数组的遍历器对象。

    任何数据结构只要有Iterator接口，就可以被 `yield*`遍历。

    [^被yield*遍历]: 如果把yield something 看成一个代码块，那么yield* 就是代码块的集合.每次执行obj.next( ) 移动遍历器指针，移到yield* 时会停留多次，直到代码块集合都被遍历完为止。

35. 被代理的Generator函数中有return语句时，可以向代理它的Generator函数返回数据，具体怎么处理呢？

    ```javascript
    function* foo() {
      yield 2;
      yield 3;
      return "foo";
    }
    function* bar() {
      yield 1;
      var v = yield* foo();
      console.log(v);
      yield 4;
    }
     let i = 1;
        let it = bar()
        while(i < 7){
            console.log(`打印第${i}次`)
            console.log(it.next())
            i ++
        }
    ```

    函数foo的return语句向bar提供了返回值

    第4次打印时，会先 打印出 foo，然后打印  {value: 4, done: false} 

36. 用`yield*`写个方法，取出嵌套数组的所有成员

    ```javascript
    function* iterTree(data){
        if(!Array.isArray(data)){
            yield data
        }else{
            // 这里遍历数组，不能用forEach，因为yield表达式不能放在普通函数里
           for(let i = 0; i < data.length; i ++){
               yield* iterTree(data[i])
           } 
        }    
    }
    let tree = ["a","b",[1,2,[3,4,[5]]]]
    console.log([...iterTree(tree)]) // ["a", "b", 1, 2, 3, 4, 5]
    ```

    上面的方法可以展开任何层级的数组

    实现的思路是：

    + data不需要继续遍历时，直接yield data
    + data需要继续遍历时，用yield* 再遍历一次  yield* iterTree(data[i])

37. 用 `yield*`遍历二叉树

    ```javascript
    function Tree(left, label, right){
        this.left = left;
        this.label = label;
        this.right = right;
    }
    // 中序遍历
    function* inorder(t){
        if(t){
            yield* inorder(t.left);
            yield t.label;
            yield* inorder(t.right);
        }
    }
    ```

    

38. 对象属性的Generator函数，如何简写？

    ```javascript
    let obj = {
    	* generatorMethod(){
            
        }
    }
    ```

    完整形式：

    ```javascript
    let obj = {
        generatorMethod: function* (){
            
        }
    }
    ```

39. Generator函数G的返回值g（iterator遍历器）和G是什么关系？

    g是G的一个实例。会继承G的prototype对象上的方法。

    ```javascript
    function* G(){
        
    }
    G.prototype.hello = function(){
        return "hi"
    }
    let g = G()
    console.log(g.hello()) // hi
    ```

40. g是G的实例，那G和普通的构造函数F有什么区别？

    两点：

    + G返回的**总是**遍历器对象，F默认返回的是this对象。在G里面，给this绑定属性，g是获取不到的
    + G不能跟new命令一起使用，会报错

41. 写一个方法，实现Generator函数返回一个正常的对象实例，既可以用next方法，也可以用正常的this？

    ```javascript
    function* G(){
        this.a = 1;
        yield this.b = 2;
        yield this.c = 3;
    }
    function F(){
        return G.cal(G.prototype)
    }
    let f = new F()
    console.log(f.next())
    console.log(f.next())
    console.log(f.next())
    // f的prototype属性（一个对象）上还有a，b，c三个属性
    ```

    当yield后面跟的是一个赋值语句时，用next方法返回的结果对象的value属性的值是此赋值语句等号右边的值。比如：

    `yield this.b = 2;`，当`f.next()`遍历到这一句时，返回的是 `{value: 2, done: false}`。

    此时做了两个操作:

    + 返回`{value: 2, done: false}`
    + 给b赋值  `this.b = 2`

42. Generator函数的应用场景有哪些？

    + 异步操作同步化（比如用 同步的写法，写ajax请求）

      思路是： 用yield表达式，让程序暂停在ajax请求这一步，在请求成功的回调里，用`g.next(response)`，把请求的结果返回出来，赋值给`yield`表达式的左边

      ```javascript
      function* main(){
          let result = yield request(someUrl)
          let resp = JSON.parse(result)
          console.log(resp.value)
      }
      function request(url){
          makeAjaxCall(url, function(){
              it.next(response)
          })
      }
      let it = main()
      it.next()
      ```

      两个next(  )方法。第一个启动yield后面的 request(someUrl)。第二个将ajax的请求结果返回出来，给result

    + 控制流管理（多个同步请求，前面的结果是后面的请求参数）

      ```javascript
      function* longRunningTask(value1){
          try{
              var value2 = yield step1(value1);
              var value3 = yield step2(value2);
              var value4 = yield step3(value3);
              var value5 = yield step4(value4);
              // do something
          }catch(e){
              // handle any error from step1 through step4
          }
      }
      scheduler(longRunningTask(initialValue))
      function scheduler(task){
          let taskObj = task.next(task.value)
          if(!taskObj.done){
              task.value = taskObj.value
              scheduler(task)
          }
      }
      ```

    + 部署Iterator接口

      Generator函数中yield关键字后面的值可以直接通过for...of遍历出来。根据这个思路可在对象、数组等上面部署Iterator接口

      ```javascript
      function* iteratorForObj(obj){
          let keys = Object.keys(obj)
          for(let i = 0; i < keys.length; i ++){
              yield [keys[i], obj[keys[i]]]
          }
      }
      let datas = {name: "chen", age: "30", height: "172cm"}
      for(let [key, value] of iteratorForObj(datas)){
          console.log(`key是${key},value是${value}`)
      }
      // 依次打印 key是name,value是chen 等
      ```

      

    + 作为数据结构

      Generator函数的返回值可以为任何表达式，提供类似数组的接口。

      ```javascript
      function* doStuff(){
          yield fs.readFile.bind(null, "hello.txt")
          yield fs.readFile.bind(nul, "world.txt")
          yield fs.readFile.bind(nul, "and-such.txt")
      }
      for(let task of doStuff()){
          //task是个函数，可认为是回调函数
      }
      ```

## Generator函数的异步应用

1. 传统异步调用有哪些？

   四种：回调函数，事件监听，发布/订阅，Promise对象

2. Generator有什么特点？用它实现异步操作有什么好处？

   在yield处暂停，等到执行权返回，再从暂停的地方继续往后执行。用Generator的优点是代码的写法像同步操作。

3. Generator函数异步任务的的思路？

   关键是函数的返回值（指针对象，也叫遍历器）的next方法。

   next方法执行过程： 调用指针g的next方法，会移动内部指针，指向第一个遇到的yield语句，并把yield语句后面的内容执行完。

   next方法返回值：包含value和done属性的对象。value是yield后面表达式的值，是当前阶段的值，done是布尔值，表示是否执行完毕，即是否还有下一个阶段。

4. Generator能完美封装异步任务的原因？

   + 可以暂停和恢复执行（根本原因）
   + Generator函数体内外可以进行数据交换
   + Generator函数有错误处理机制

5. Generator函数内外是如何进行交换？

   ```javascript
   function* gen(x){
       var y = yield x + 2;
       return y;
   }
   var g = gen(1)
   console.log(g.next())
   console.log(g.next(2))
   ```

   内 =》 外： `yield`是Generator函数内部的关键字，它后面表达式的值，会作为`next`方法的返回值（含value和done的对象）的value属性，传出来。执行第一个`g.next()`时，得到的对象中 value就是 1+2

   外 =》 内：`next()`是Generator函数外部的方法，它里面的参数，会作为参数传给上一个yield表达式左边的变量（如果左边变量有的话）。所以第二个执行next，`g.next(2)`时，会在上一个yield处恢复执行，先把2赋给y，然后继续往下走。

6. Generator函数体内部如何捕获函数体外的代码？

   Generator函数体外，用`iterator`对象的`g.throw("出错了")`抛出错误

   Genernator函数体内，用`try...catch`代码块捕获异常

7. 什么是Thunk函数？

   在“传名调用”中将参数放到一个临时函数之中，再将这个临时函数传入函数体。这个临时函数就加做Thunk函数。例子：

   ```javascript
   //前置条件，定义了函数f(m)，调用了f(x+5).
   // 相当于
   var thunk = function(){
       return x + 5;
   }
   function f(thunk){
       return thunk() * 2;
   }
   ```

   

8. JavaScript语言中的Thunk函数有什么不同？

   替换的不是表达式，而是多参数函数，将其替换成一个只接受回调函数作为参数的单参数函数。

   ```javascript
   // 原始版，接收两个参数
   fs.readFile(fleName, callback)
   //Thunk版
   var Thunk = function(fileName){
       return function (calback) {
           return fs.readFile(fileName, callback)
       }
   }
   var readFileThunk = Thunk(fileName)
   readFileThunk(callback)
   ```

9. co模块原理：后面再补充

## async函数

1. async是什么？它和Genernator函数外观上的区别是什么？

   ```javascript
   const asyncReadFile = async function(){
       const f1 = await readFile(url1)
       const f2 = await readFile(url2)
   }
   ```

   async是Genernator函数的语法糖。

   `async`和`Genernator`的区别有两点：

   + `Genernator`的 `*`编程`async` 
   + `yield`变成  `await`

2. `async`对`Genernator`做了哪些改进？

   + 内置执行器。

     执行时和普通函数一样，只需要调用函数就完了，不需要 `next()`方法

   + 更好的语义。

     `async`表示它后面的函数里有异步操作；`await`表示紧跟在后面的表达式需要等待结果

   + 更广的适用性。

     await后面可以`Genernator`Promise对象和原始类型的值。（原始类型的值会被自动转化为resolved状态的Promise对象）

   + 返回值是Promise

     `async`函数返回的是`Promise`对象，方便用 `then`进行下一步操作

   总的来说：`async`函数可看成多个异步操作，包装成一个`Promise`对象，而`await`命令就是内部`then`命令的语法糖。

3. `async`执行的例子：

   ```javascript
   function timeout(ms){
     return new Promise((resolve) => {
       setTimeout(resolve, ms)
     })
   }
   async function asyncPrint(value, ms){
     await timeout(ms);
     console.log(value)
   }
   
   asyncPrint("你好啊", 5000)
   ```

   5s之后打印 “你好啊”  

   以上 `asyncPrint`的意思是，等待timeout函数执行完了之后，才会继续执行 `console.log(value)`

4. async函数的几种使用场景？

   五种场景下：

   + 函数声明

   ```javascript
   async function foo(){}
   ```

   + 函数表达式

   ```javascript
   const foo = async function(){}
   ```

   + 箭头函数

   ```javascript
   const foo = async () => {}
   ```

   + 对象的方法

   ```javascript
   let obj = {
   	async foo(){
           
      	}
   }
   obj.foo().then(...)
   ```

   + class方法

   ```javascript
   class Storage{
   	constructor(){
           this.cachePromise = caches.open("avatars")
       }
       async getAvatar(name){
           const cache = await this.cachePromise;
           return cache.match()
       }
   }
   const storage  = new Storage()
   storage.getAvatar("joy").then(...)
   ```

   

5. async函数返回什么？

   返回一个 `Promise`对象。

   `async`函数内部 return语句的返回值，会成为`then`方法回调函数的参数。

   `async`函数内部抛出的错误，会导致返回的`Promise`对象变为 `rejected`状态

6. ``async`函数中`Promise`对象是如何变化的？

   `async`函数返回的是`Promise`对象P。必须等到内部 `await`命令的`Promise`对象执行完后，P才会发生状态改变，除非遇到return语句，或者抛出了错误。

   换言之，`async`函数 内部的异步操作执行完了，才会执行调用它时后面`then`方法指定的回调函数。

7. `await`命令的返回值是什么？

   + 正常情况下，`await`后面是`Promise`对象，会返回此对象的结果；如果不是`Promise`对象，直接返回对应的值
   + `await`命令后面跟了 `thenable`对象，会把 `thenable`对象当做 `Promise`对象来处理

8. `await`后面的`Promise`对象如果变为 `rejected`会怎样？

   + rejected的参数会被`async`函数catch方法的回调函数接收到。

     ```javascript
     async function f() {
       await Promise.reject('出错了');
     }
     
     f()
     .then(v => console.log(v))
     .catch(e => console.log(e))
     ```

   + 任何一个`await`语句的`Promise`对象变为reject状态，整个`async`函数都会被中断

     ```javascript
     async function f() {
       await Promise.reject('出错了');
       await Promise.resolve('hello world'); // 不会执行
     }
     ```

9. 如果希望前一个异步操作失败，不中断后面的异步操作，怎么处理？

   两种方法：

   + 把第一个`await`放到 `try...catch`里面

     ```javascript
     async function f(){
     	try{
             await Promise.reject("出错了")
         }catch(e){}
         return await Promise.resolve("hello world")
     }
     f().then(v =>console.log(v)).catch(e => console.log(e))
     // 打印的是  hello world
     ```

     

   + `await`后面的`Promise`对象再跟一个 `catch`方法，处理前面可能出现的错误

     ```javascript
     async function f(){
       await Promise.reject("出错啦").catch(e => {
         console.log("await 内部promise被reject了")
       })
       return await Promise.resolve("hello world");
     }
     f().then(v => console.log(v)).catch(e => console.log(e))
     ```

     打印的内容是：

      await 内部promise被reject了
        hello world

10. await后面的异步操作出错了（例如某行代码throw 了一个Error），是什么意思？

    `async`函数返回的`Promise`对象被`reject`了

    ```javascript
    async function f() {
      await new Promise(function (resolve, reject) {
        throw new Error('出错了');
      });
    }
    
    f()
    .then(v => console.log(v))
    .catch(e => console.log(e)) // catch执行了， e就是抛出的错误对象 new Error('出错了')
    ```

    

11. 如何防止出错呢？

    还是将其放到`try{ }catch(e){ }`代码块中。

    如果有多个`await`命令，可以将其同意放到`try{ }catch(e){ }`结构里

    ```javascript
    async function test(){
      let i;
      for(i = 0; i < NUM_RETRIES; ++i){
        try{
          await superagent.get(url)
          break;
        }catch(err){}
      }
      console.log(i)
    }
    test()
    ```

    如果`await`操作成功，则会break，跳出for循环；如果`await`操作不成功，则会被catch住，然后继续下一轮for循环，直到超过 NUM_RETRIES或者 `await`操作成功。

12. `async`和`await`有哪些使用上注意的点？

    + `await`命令后的`Promise`对象可能`reject`，因此`await`命令最好放在`try{ }catch(e){ }`代码块中

      ```javascript
      async function myFunction() {
        try {
          await somethingThatReturnsAPromise();
        } catch (err) {
          console.log(err);
        }
      }
      
      // 另一种写法
      
      async function myFunction() {
        await somethingThatReturnsAPromise()
        .catch(function (err) {
          console.log(err);
        });
      }
      ```

      

    + 多个`await`异步操作时，如果不存在继发关系，让它们同时触发比较好

      可以结合 `Promise.all`方法

      ```
      // 写法一
      let [foo, bar] = await Promise.all([getFoo(), getBar()]);
      
      // 写法二
      let fooPromise = getFoo();
      let barPromise = getBar();
      let foo = await fooPromise;
      let bar = await barPromise;
      ```

      

    + `await`命令只能用在`async`函数中，用在普通函数里会报错

      注意`forEach`的回调函数，`await`也不能出现在回调函数里

    + `async`函数可以保留运行堆栈

      看例子：

      ```javascript
      const a = () => {
          b().then(() => c()) ;
      }
      
      const a = async () => {
          await b();
          c();
      }
      ```

      上面的例子中，b运行时，a可能已经执行完了。如果此时b或c报错，错误堆栈将不包括a

      下面例子中，b运行时，a只是暂停，若此时b或者c报错了，错误堆栈中将包括a

13. 与`Promise`写法和`Genernator`写法，`async`有什么好处？

    `Promise`写法有很多`catch`和`then`，语义性不强。

    `Genernator`函数需要有一个任务运行器，自动执行`Genernator`函数，并且 `yield`后面的表达式，必须返回`Promise`对象

    `async`最简洁，最符合语义，将`Genernator`写法的自动执行器，改在 语言层面提供，不暴露给用户，代码量最少。

    ```javascript
    async function chainAnimationAsync(elem, ainmations){
        let ret = null
        try{
            for(letanim of animations){
                ret = await anim(elem);
            }
        }catch(e){}
        return ret
    }
    ```

    

14. `async`的实例： 按顺序完成异步操作——依次远程读取一组URL，然后按照读取顺序输出结果

    ```javascript
    async function logInOrder(urls){
      const textPromises = urls.map(async url => {
        const response = await fetch(url)
        return response.text()
      })
      for(const textPromise of textPromises){
        console.log(await textPromise)
      }
    }
    ```

    map的参数是`async`函数。这几个`async`是并发的。只有`async`函数内部才是继发的【`const response = await fetch(url)` 比 `return response.text()` 先执行】，外部并不受影响。

    后面在 `for...of`循环内部使用了 `await`，这几个`await`是顺序执行。

15. 顶层`await`的用法和场景、后面再补充。

##  clsss用法：

前置例子：

```javascript
class Point {
    constructor(x,y){
this.x = x
this.y = y // 实例自己的
    }
    toString(){
return `${this.x}, ${this.y}` // 类的
    }
}
// 实例
let point = new Point(3, 4)
```



1. 实例是什么类型？类和实例有什么关系？

   实例全称是类的实例。类相当于实例的构造函数。但是和构造函数相比，只能通过new方法调用，不能自执行。构造函数就是普通函数加一个new修饰符。

   ```javascript
   point.__proto__ === Point.prototype // true
   ```

   以上等式在构造函数和实例之间，也是成立的。如下：

   ```javascript
   let f = function (){
   this.a = 1
   this.b = 2
   }
   
   let o = new f()
   o.__proto__ === f.prototype //true
   ```

   

2. 实例的属性哪些是自己的，哪些是原型的？判断属性是否是自己的用对象的什么方法？

   定义在this上的属性就是自己的。其他的，就是原型的。

   用hasOwnProperty( )方法判断属性是否在对象自身

   ```javascript
   point.hasOwnProperty("toString") // false
   point.__proto__.hasOwnProperty("toString") // true
   ```

   

3. 实例如何访问定义在类上的方法？

   类上定义的方法，实例可以直接访问

   ```javascript
   point.toString() // "(3,4)"
   ```

   

4. 如何给类的某个属性设置存值和取值函数？

   在类的内部使用set和get，对某个属性设置存值函数和取值函数

   ```javascript
   class MyClass {
   constructor() {
    // ...
   }
   get prop() {
    return 'getter';
   }
   set prop(value) {
    console.log('setter: '+value);
   }
   }
   ```

   

5. 类的属性名表达式是什么，怎么写的？

   类的属性名用表达式来写。将变量写在方括号里，如下:

   ```javascript
   let methodName = "something"
   [methodName](){
   
   }
   ```

   

6. 类可以用表达式形式定义吗？写一个立即执行的类

   可以。

   ```javascript
   let person = new class Me{
   constructor(name){
   this.name = name
    } 
    sayName(){
   consle.log(this.name)
    }
    }("joy")
   ```

    

7. ES6内部还需要用use strict吗？

   不需要。ES6 实际上把整个语言升级到了严格模式。

8. 类有变量提升吗？

   没有。因此子类要定义在父类后面。

9. 类的name属性怎么取？ 类对外是用的什么符号？

   name是class后面的变量名。类对外的符号是等号左边的变量。

   ```javascript
   let myPoint = class Point{}
   myPoint.name  // Point
   // myPoint指代了Point类
   ```

   

10. 如何判别Generator方法？

    某方法之前加了星号（*），表示该方法是geneator函数

11. 类的方法中的this指向的是什么？如果将方法单独提出来呢？怎么让this指向实例？

类的方法的this指向的是类实例。如果单独提出来指向的是undefined。让this指向实例有3种方法： ①构造函数绑定this（函数的bind方法） ②用箭头函数  ③用proxy，获取方法的时候，自动绑定this

12. 什么是静态方法，举例说明？如何调用静态方法？

一个方法前加了static，它就是静态方法。如下：

```javascript
class Foo{
 static classMethod(){
return "hello"
 }
}
Foo.classMethod() //静态方法在类上调用
```

静态方法直接通过了来调用

13. 类中定义的方法，都会被实例继承吗？

类中定义的非静态方法都会被实例继承。

14. 静态方法中的this，指向什么？

指向类

15. 实例的属性除了写在constructor中，还能写在哪里？

写在constructor 或者写在顶层，效果一样。

16. 静态属性如何定义？

写在类的外面 `Foo.prop = 1`或者在属性前加static修饰符  `static prop= 2 `

17. 什么是私有属性和私有方法? 如何区别他们和别的属性或方法。

私有方法和私有属性，是只能在类的内部访问的方法和属性，外部不能访问。私有属性的方法：

①命名区别  ②方法移除到模块外  ③利用`Symbol`值的唯一

私有属性的提案：

在属性和方法前面加#，表示私有

18. new.target属性的应用场景？

ES6 为new命令引入了一个new.target属性，该属性一般用在构造函数之中，返回new命令作用于的那个构造函数。

能写出不能独立使用、必须继承后才能使用的类

```javascript
class Shape {
  constructor() {
 if (new.target === Shape) {
   throw new Error('本类不能实例化');
 }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
 super();
 // ...
  }
}

var x = new Shape();  // 报错
var y = new Rectangle(3, 4);  // 正确
```



## class的继承：

前置方法

```javascript
    class Point {
 constructor(x, y) {
this.x = x
this.y = y
 }
 toString() {
return `(${this.x},${this.y})`
 }
 /* 静态方法 */
 static hello() {
console.log("hello world!")
 }
    }

    // 子类实例的构建，基于父类实例
    // super 方法调用父类的实例。使用了super后，才能用this
    class ColorPoint extends Point {
 constructor(x, y, color) {
super(x, y) // super作为方法，用在子类的构造,必须在this之前先调用
this.color = color
 }
 toString() {
//  super作为对象（对象调用方法），指向的是父类的原型对象即Point.prototype
console.log(Point.prototype.toString === super.toString) // true
return `color: ${this.color}, ${super.toString()}`
 }
    }
```

1. 子类和父类是通过什么关键字实现继承？父类是Point，子类是ColorPoint如何写？

   通过extends关键字 

   ```javascript
   class ColorPoint extends Point{
    
   }
   ```

2. 子类的属性和方法中，哪个关键字是必须的？这个关键字怎么用？为什么要这么用？

   super关键字必须。

   super关键字有两种用法，作为方法和作为对象。

   作为方法时，用在constructor方法中（用于子类的构造），必须在this之前调用。

   作为对象时，用在其他方法中，调用父类的方法用。指向的是父类的原型对象。

   ```javascript
   Point.prototype.toString === super.toString // true
   /*
   super作为对象时，不能不调用方法，直接拿来用，比如  Point.prototype === super 就会控制台报错
   */
   ```

   

3. `ColorPoint.hello()`会输出什么？为什么？

   会输出 hello world!  因为`ColorPoint`是`Point`的子类，会继承它的所有方法。`hello()`是`Point`的静态方法，也会被子类`ColorPoint`继承。

4. 如何从子类获取父类？代码演示

   ```javascript
   Object.getPrototypeOf(ColorPoint) === Point   //true
   ```

   用`Object.getPrototypeOf(childClass)` 的方法可以判断一个类是否继承了另一个类

5. 简述一下super的两种使用场景

   super可作为方法，也可作为 对象

   方法：子类的constructor中使用。

   对象：在子类的其他方法中使用。普通方法  = >  指向父类原型对象（父类的方法全部定义在原型对象上）；   静态方法  =>  指向父类

6. 子类的`__proto__`属性，指向的是什么？

   子类的`__proto__`属性指向**父类**

   ```javascript
   colorPoint.__proto__ === Point // true
   // 构造函数的继承
   ```

7. 子类的prototype属性的`__proto__`属性指向什么?

   子类的prototype属性的`__proto__`属性指向父类的prototype属性

   ```javascript
   colorPoint.prototype.__proto__ === Point.prototype  // true
   // 类的方法的继承
   ```

   

8. 原生构造函数有哪些？

   ```javascript
   Boolean()  
   Boolean 对象是一个布尔值的对象包装器。当需要判断一个对象是正负时，可以这么做
   将非布尔值转化为布尔值，用Boolean方法，Boolean(exp) 或者 !!(exp)
   ```

   ```javascript
   Number()
   
   ```

   ```javascript
   String()
   
   ```

   ```javascript
   Array()
   
   ```

   ```javascript
   Date()
   
   ```

   ```javascript
   Function()
   
   ```

   ```javascript
   Regexp()
   ```

   ```javascript
   Error()
   ```

   ```javascript
   Object()
   ```

   

9. ES5的方法实现继承，可以继承到原生的构造函数吗？为什么？

   ES5是先新建子类的实例对象this，再将父类的属性添加到子类上。由于父类的内部属性无法获取，导致无法继承原生的构造  函数。

10. ES6的方法实现继承，可以继承到原生的构造函数吗？为什么？

    ES6 是先新建父类的实例对象this，然后再用子类的构造函数修饰 this，使得父类的所有行为都可以继承。

    
    
    