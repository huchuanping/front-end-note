### 1. 让图片变黑白
```
img{
    filter:grayscale(100%);
    -webkit-filter:grayscale(100%);
    -moz-filter:grayscale(100%);
    -ms-filter:grayscale(100%);
    -o-filter:grayscale(100%);
}
```
### 2. 使用伪类:not()在菜单上应用/取消应用边框
菜单
```
.nav li{
    border-right:1px solid #666;
}
```
然后去除最后一个元素
```
.nav li:last-child{
    border-right:none;
}
```
上面是普通做法，用not()伪类应用元素：
.nav li:not(:last-child){
    border-right:1px solid #666;
}
### 3. 给body添加行高
不需要给每个p、h标记添加行高，只需要添加到body上即可：
```
body{
    line-height:1;
}
```
### 4.让一切元素都垂直居中 
```
html,body{
    height:100%;
    margin:0;
}
body{
    -webkit-align-items:center;
    -ms-align-items:center;
    align-items:center;
    display:-webkit-flex;
    display:flex;
}
```
### 5.表格单元格等宽 
```
.calendar{
table-layout:fixed
}
```
### 6.禁用鼠标事件 
.disabled{
    pointer-events:none
}
### 7.模糊文本 
.blur{
    color:transparent;
    text-shadow:0 0 5px rgba(0,0,0,0.5)
}