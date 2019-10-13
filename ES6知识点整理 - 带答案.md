## let和const：

1.let有哪三点需要注意的？

①    无变量提升  ②需要先声明再使用（存在一个暂时死去）③相同作用域内不能重复声明 ④如果外层声明了变量，内层用let重新声明，那么变量属于内层。所以变量使用仍然应在声明之后，否则报错。

2.const和let有哪些异同？如何让const变量的属性也不让变？Object.freeze怎么用？

①    同：都有作用域、相同作用域内不能重复声明、无变量提升  

②    异：1，const声明的数值变量不能变本质是变量指向的内存地址保存的数据不变（数值，对象地址、数组地址）,2，const的声明和赋值必须一起 

③    变量的属性也不让变？const声明时，将对象冻结，用Object.freeze  const foo = Object.freeze({ });  这种冻结在严格模式下生效，一般模式下不生效

④    Object.freeze，用于

3.es6有几种变量声明方法，分别是什么？

function var let const import export   一共6种方法

## 字符串扩展：

1.ES5中，如何用unicode码表示一个字符？在es6中做了什么改进？为什么要做这个改进？

①    es5中，unicode码表示一个字符的方法：\uxxxx 的方式表示一个字符，可以表示\u0000到\uFFFF之间的字符，超过FFFF的只能用双字节来表示

②    es6做的改进，\u{20BB7}

③    这样做可读取码点超过\uFFFF的字符

2.ES5是以什么格式存储字符的，每个字符多少字节？对于4字节的字符，在ES6中是用哪一个新增的方法处理的？

①    ：UTF-16 的方式，存储字符，一个字符2字节。

②    4字节的字符，es6中codePointAt方法处理

3.codePointAt返回的是码点是多少进制的？如何转化为16进制？

①    codePointAt返回的码点是十进制

②    转化为16进制，toString一下

4.如何解决codePointAt在使用时如果有4字节字符，码点不对的问题？

①    用for…of循环

**let** s **=** '𠮷a'**;**

**for** **(****let** ch **of** s**)** **{**

  console**.**log**(**ch**.**codePointAt**(**0**).**toString**(**16**));**

**}**

5.如何检测一个字符是2字节还是4字节？请写函数is32Bit(c).

**function** is32bit**(**ch**){**

​    **return****(**ch**.**codePointAt**(**0**)>** 0xFFFF**);//****测试有效**

**}**

6.es5如何从码点返回一个对应的字符串？这个方法有什么局限？es6又是如何实现的？

①    es5中，码点返回字符串，formCharCode

②    局限：不能返回32位的utf-16字符

③    es6实现：fromCodePoint实现

7.简述一下startsWith（param1，param2），endsWith（param1，param2）和includes（param1，param2）用法？

①    s.startsWith（str,index） 从字符串s的index位置开始，向后找str，能马上找到返回true，否则false

②    s.endsWit(str,index) 从字符串s的index-1位置开始，向前找str，能马上找到返回true，否则false

③    s.includes（str,index）从是的index位置开始找，能否找到str，能找到返回true，不能，返回false

8.ES6将字符转化为为码点的方法  codePointAt  ，将码点转化为字符 fromCodePoint 

9.如何将字符串重复n次，用的是什么函数？此函数的param取正整数，非正整数，字符串，布尔值时，分别表示重复了多少次？

①    重复n次：repeat函数。使用方法：str.repeat(count)  str重复count次。

②    浮点数时，count会取整；非负数字符串时，count=0；布尔值true时，count=1；布尔false时，count=0 ；负数或负数字符串是时，报错

10.头部补全和尾部补全，用的是什么方法？用补全的方法，写一个例子，取当天日期，格式是‘yyyy-mm-dd’

①    头部补零：s.padStart(len,str)：将字符串s头部用str补充，直到长度是len，返回新字符串

②    尾部补零：s.padEnd(len,str)：将字符串s尾部用str补充，直到长度是len，返回新字符串

**function** getToday**(){**

​    **var** now **=** **new** Date**();**

​    **let** year **=** now**.**getFullYear**();**

​    **let** month **=** **(**now**.**getMonth**()+**1**+**''**).**padStart**(**2**,**'0'**);**

​    **let** day **=** **(**now**.**getDate**()+**''**).**padStart**(**2**,**'0'**);**

​    console**.**log**(**year**+**'-'**+**month**+**'-'**+**day**);**

**}**

 

11.反引号是怎么用的？用在什么场合？（两个基本使用方法）

①    `  `

②    场合：换行和赋值  换行时，可在两个反引号中写html代码。赋值时使用美元符+花括号的形式，如：${ param }

12.数字转化为字符串有哪几种方法？（三种）

toString()方法： num.toString() num为null或者undefined时，会报错

String()方法: String(num)  num为null时，转化为“null”，为undefined时，转化为“undefined”

num+” ” : 结果同String( )方法

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
   ```
[proKey]:true
   } //{kkkk: true}
   ```

   ```javascript
   // 定义方法名
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
   
   ```

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

    ②为对象添加方法
    
    例如给prototype属性添加方法
    
    ```JavaScript
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

​    

17. 如何获取对象属性的描述对象？

    用Object.getOwnPropertyDescriptor

    `Object.getOwnPropertyDescriptor(objEight,'foo')`

18. 对象`objEight = {foo:123}`， 属性foo的描述对象包含哪4个属性？

    ```javascript
    {value: 123, writable: true, enumerable: true, configurable: true}
    ```

19. `enumerable`是描述对象的什么属性？`Object.keys(  )`跟它有什么关系？

    `enumerable`是可枚举属性。为`true`时表示可枚举，为`false`时表示不可枚举。

    `Object.keys(  )`遍历对象时会忽略掉不可枚举的属性。其他几个和`Object.keys(  )`有相同表现形式的有 `for ...in`,`Object.keys(  )`,`JSON.stringify()`,`Object.assign()`

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

20. 属性有哪些遍历方法？

    四种方法。

    ①   Object.keys(  )

    ②   for ... in // 继承属性也会遍历出来

    ③   Object.getOwnPropertyNames(obj)  // 和Object.keys(  )结果一样

    ④   Object.getOwnPropertySymbols(obj)  //是 [ ]

    ⑤   Reflect.ownKeys(obj)  // 和Object.keys(  )结果一样

    

21. 属性的遍历规则是什么？（从数值，字符串，symbol考虑）

    数值键： 按升序排列

    字符串 ：按加入时间升序

    Symbol键：按照加入时间升序

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

    都是可遍历属性；不含继承属性；都会过滤掉Symbol值的属性

27. Object.keys()， Object.values()和Object.entries()返回值分别是什么？给出他们三个和解构赋值，for...of一起用的例子？

    ```javascript
    let { keys, values, entries } = Object
    for(let key of entries(obj)){
    consle.log(key)
    }
    ```

    

28. ``Object.entries()`有哪两种用途？

    1. 遍历对象，得到`[key, value]`键值对组成的数组

    2. 将对象转化为真正的map解构。看例子：

  ```javascript
  const obj = {foo: "bar", baz:42}
  const map = new Map(Object.entries(obj))
  ```

  

28. 对象中可以用扩展运算符吗？

    可以用。但是扩展运算符只能用在尾部。看例子：

    ```javascript
    let { x13, y13, ...z13} = { x13: 1, y13: 2, a13: "c", d13: "b"}  
    // z13的值是：{a13: "c", d13: "b"}
    ```

    

29. 扩展运算符能不能继承原型对象的属性？

    不行。扩展运算符只能复制对象自身的属性，不能复制继承属性。例如下面代码中，obj有b属性，但是targetObj没有。因为b属性是继承自obj的原型对象proObj。

    ```javascript
    let obj = {a: 1}
    let proObj = {b: 2}
    Object.setPrototypeOf(obj, proObj)
    let {...targetObj} = obj
    obj.b  //2
    targetObj.b // undefined
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

    `Set.prototype[Symbol.iterator] === Set.prototype.values`  // true

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
    map[Symbol.iterator] === map.entries  //true
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
    // Map的forEach()方法: 两个参数，一个回调fn(key, value, map) 一个		thisArg(this指向的对象)
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

1.数组解构赋值的匹配模式是怎样的？

①    数组匹配模式：左边和变量和右边数组的数据，按顺序匹配

2.如何完全解构，获取数组的第一个和最后一个元素 [1,2,3,4]?

①    完全解构获取第一个和最后一个元素。 let [x21,,,x22] = [1,2,3,4]

(中间不需要的参数，全部空出来)

3.如何用不完全解构，获取数组的第一个和第二个元素[1,2,3,4]？

①    不完全解构。let [x31,x32] = [1,2,3,4]  

4.数组解构时，默认值是表达式时有什么特点？

①    默认值是表达式时，惰性赋值。只有取到默认值时，表达式才会执行。不取默认值时不执行。

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

①命名区别  ②方法移除到模块外  ③利用Symbol值的唯一

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

    
    
    