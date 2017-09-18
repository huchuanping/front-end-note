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
