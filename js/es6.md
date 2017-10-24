## let和const命令
### let
    let声明的变量只在let所在的代码块有效
    ```
      {
        var a=5
        let b=10
      }
      a //5
      b //ReferenceError: b is not defined.
    ```
    var 声明的变量会被提升到顶部，let不会，同一会代码块作用域不允许重复声明变量，
    父作用域声明不影响子作用域。
    ```
    //报错
      {
        let a=10;
        let a=3;
      }
    ```
    外层代码块不受内层代码块的影响
    ```
    function f1(){
      let n=5
      if(true){
        let n=10
      }
      console.log(n)//5
    }
    ```
### const命令
    const用于声明一个只读的常量，一旦声明常量值就不可以改变了
    一个常量被声明的同事还必须要赋值，否则会报错,同样只在当前作用域内生效
    ```
    const PI=3.1415
    ```

## 变量的解构赋值
定义：es6允许按照一定模式，从数组和对象中提取值，对变量进行赋值
其实就是模式匹配，等号左右两边模式相同，左边的变量就会被赋予对应的值
### 数组的解构赋值
数组元素按此排列，变量的取值由元素位置决定
```
let [a,b,c]=[1,2,3]
a //1
b //2
c //
let [,,third]=['foo','bar','baz']
third //'baz'
let [x,,y]=[1,2,3]
x//1
y//3
let [head,...tail]=[1,2,3,4]
head//1
tail//[2,3,4]
let [x,y,...z]=['a']
x//'a'
y//undefined,解构不成功就是undefined
z//[]
```
### 对象的解构赋值
变量的取值由相同的属性名决定，与次序
```
let {bar,foo}={foo:"aaa",bar:"bbb"}
foo //"aaa"
bar //"bbb"
let {baz}={foo:"aaa",bar:"bbb"}
baz //undefined
```
对象的解构赋值的内部机制是先找到同名属性，然后在付给对应的变量，真正被赋值的是后者
```
let {foo:baz}={foo:"aa",bar:"bb"}
baz//"aa"  baz才是真正的变量
foo //error: foo is not defined
```
### 字符串解构赋值
字符串先被转换成一个类似数组的对象，类似数组的对象都有一个`length`的属性,因此可以对该属性解构赋值
```
const [a,b,c,d,e]='hello'
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
let {length:len}='hello'
len //5
```
### 解构赋值用途
* 变量值交换
```
[x,y]=[y,x]
```
* 从函数返回多个值
```
//返回一个数组
function exzmple(){
  return [1,2,3]
}
let [a,b,c]=exzmple()
//返回一个对象
function example(){
  return {
    foo:1,
    bar:2
  }
}
let {foo,bar}=example()
```
* 函数参数的定义
```
//参数是一组有次序的值
function f([x,y,z]){...}
f([1,2,3])
//参数无次序
function f({x,y,z}){}
  f({z:3,y:2,x:1})
```
* 提取JSON数据
```
let jsonData={
  id:11,
  status:'ok',
  data:[777,333]
}
let {id,status,data:number}=jsonData
console.log(id,status,number)//11,'ok',[777,333]
```
## 字符串扩展
### 模板字符串
用反引号(` `) 表示模板字符串
模板字符串中嵌入变量，需将变量名写在`${}`中
```
var temp=`There are <b>${basket.count}</b> items in
your basket
`
var x=1
var y=2
`${x}+${y}=${x+y}`//"1+2=3"
```
模板字符串中还能调用函数
```
function fn(){
  return 'hello'
}
`foo ${fn()} bar`
```
## 正则扩展
## 数值的扩展
## 函数的扩展
## 数组的扩展
* 扩展运算符 ...
扩展运算符用三个点表示，将一个数组或类数组对象转为用逗号分隔的参数序列
```
console.log(...[1,2,3])
//1 2 3
```
1. 运算符主要用于函数调用
```
function add(x,y,z){
  return x+y+z
}
var numbers=[2,3,4]
add(...numbers)
  //9
```
2. 用push函数将一个数组添加到另一个数组尾部
```
let arr1=[1,2,3]
let arr2=[4,5,6]
arr1.push(...arr2)
```
3. 用于合并数组
```
//ES5
[1,2].concat(more)
//es6
[1,2,...more]
```
4. 与解构赋值结合可以生成数组,只能放在参数的最后一位
```
const [first,...rest]=[1,2,3,4,5]
first//1
rest//[2,3,4,5]
```
5. 帮助函数返回多个值
```
let dataFile=readDateFields(database)
let d=new Date(...dataFile)
```
上面代码从数据库取出一行数据，通过扩展运算符，直接将其传入构造函数Date。
6. 把字符串转换成数组
```
[...'hello']
//["h", "e", "l", "l", "o"]
```
7. Map结构都可以用扩展运算符
```
let map=new Map([[1,'one'],[2,'two'],[3,'three']]);
let arr=[...map.keys()]
```
## 对象的扩展
1.属性简洁表示法
es6允许直接写入变量和函数作为对象的属性和方法
```
const baz={foo:foo}
//缩写
const baz={foo}

const obj={
  method:function(){
    //body
  }
}
const obj={
  method(){
    //body
  }
}
```
2.属性名表达式
访问对象属性有两种方法`.`和方括号[]
```
obj.foo=true;
obj['a'+'bc']=123;
```
es6允许对象用表达式作为对象的属性名，表达式放在方括号内
```
let obj={
  [proKey]:true,
  ['a'+'bc']:123
}
```
3. 对象方法的name属性
name也可以用表达式表示
```
const key1
let obj={
  [key1](params){

  }
}
```
4.Object.assign(target,source1,source2)
该方法用于对象的合并，将源对象所有可枚举属性，复制到目标对象
```
const target={a:1}
const source1={b:2}
const source2={c:3}
Object.assign(target,source1,source2)
target //{a:1,b:2,c:3}
```
常见用途
* 为对象添加属性
```
class Point{
  constructor(x,y){
    Object.assign(this,{x,y})
  }
}
```
* 为对象添加方法
```
Object.assign(someObj,{
  someMethod(arg1,arg2){
    ...
  }
  })
```

## Set和Map数据结构
## Promise对象
## Iterator和for...of循环
## Generator函数
## Generator的异步
## async函数
## Class的基本语法和继承
## Module的语法
## Module的加载实现
## 编程风格
