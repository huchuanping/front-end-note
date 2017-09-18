## for...in循环 遍历对象属性
for循环不仅可以遍历数组，也可以遍历类数组的对象
```
var obj={a:[1,2],b:[3,4]}//把这个对象转换成数组
var arr=[]
for(i in obj){
  arr.push({[i]:obj[i]})
}
console.log(arr) //[{a:[1,2]},{b:[3,4]}]
```
## 普通对象和json对象区别
JSON只是一种数据格式，不是一种数据类型
```
var obj={name:'aa',age:50}//这是普通对象
var obj={'name':'aa','age':50}//这也是普通对象，因为对象的所有键名都是字符串，所以不加引号也可以
var jsonObj={"name":'aa',"age":50}//这是json格式对象（只要把普通对象的属性名用双引号包一起不能使单引号，这样的格式就是JSON格式的对象）

```
