## 2018-7-12<br>
[markdown语法](https://www.jianshu.com/p/191d1e21f7ed)<br/>
## 函数的扩展：
## 还记得哪些？
------------
1. 函数的参数默认赋值
```
function(x,y=x){
    console.log(y);
}
```
+ 在括号中给参数赋值
+ 赋值后，参数所在的是个单独作用域
+ 赋值后，函数内部，不能显示用严格模式，即写了`use strict`会报错
2. 函数的name属性
+ 匿名赋值:赋值参数名
+ 非匿名函数赋值：function后的名字
3. 函数的length，arguments属性
+ 当函数参数有默认值时，从有默认值的地方，到前面，都不算长度，因为函数参数是希望传入的参数。例子：
`function fn(x,y,z=3,r){}` fn.length = 2,fn的arguments也是{x,y}
4. 箭头函数
+ 实例
`const fn = v => v`等价于    
```
const fn = function(v){
    return v;
}
```
+ 箭头函数的this：总是指向创建时，当前对象的this，箭头函数本身无this
## 忘了哪些？
------------

    
