## 数组常用方法
### 利用reduce可以给数组分组（按照数组某个属性）
```
//已知某个数组含有多个元素，有些元素相同，要把相同的元素放在一起作为一个数组
var a=[{name:'a',id:1},{name:'a',id:1},{name:'b',id:2},{name:'b',id:2},{name:'c',id:3}]
a.reduce(function(pre,curr,index){
  pre[curr.name]=pre[curr.name]||[]
  pre[curr.name].push(curr)
  return pre
  },{})
    //输出 {a: [{name:'a',id:1},{name:'a',id:1}], b: [{name:'b',id:2},{name:'b',id:2}], c:[{name:'c',id:3}]}
```
## Array.from 从一个类似数组或可迭代的对象中创建一个新的数组实例
  类数组就是必有一个length属性的对象
  可以从下面两者来创建数组：
  * 类数组对象（拥有一个length属性和若干索引属性的任意对象）
  * 可遍历对象（Map Set）
  该方法返回一个新的数组
```
var obj={
  '0':'a',
  '1':'b',
  '2':'c',
  length:3
}
Array.from(obj)//['a','b','c']
```
### ES6 Set和Map数据结构
    new Set()可以给数组去重，返回一个新的数组
