# HTML5的sessionStorage和localStorage
html5的web Storage包括两种存储方式：sessionStorage和localStorage
** sessionStorage ** 用于本地存储一个会话中的数据，不是持久性的
** localStorage ** 是持久化的存储，除非手动删除否则一直存在
## web storage和cookie区别
cookie大小受限，多次被调用浪费宽带，另外cookie需要指定作用域不可以跨域调用。
web storage可以更大容量存储，除此之外还提供了setItem、getItem，removeItem，clear
等方法，不像cookie需要前端开发者自己封装setCookie，getCookie。
但是Cookie也是不可以或缺的：Cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，
而Web Storage仅仅是为了在本地“存储”数据而生（来自@otakustay 的纠正）
## html5 web storage的浏览器支持情况
浏览器的支持除了IE７及以下不支持外，其他标准浏览器都完全支持(ie及FF需在web服务器里运行)，值得一提的是IE总是办好事，例如IE7、IE6中的UserData其实就是javascript本地存储的解决方案。通过简单的代码封装可以统一到所有的浏览器都支持web storage。
要判断浏览器是否支持localStorage可以使用下面的代码：
```
if(window.localStorage){
    alert("浏览支持localStorage")
}else{
   alert("浏览暂不支持localStorage")
}
//或者
if(typeof window.localStorage == 'undefined'){
    alert("浏览暂不支持localStorage")
}
```
## localStorage和sessionStorage操作
存储value : setItem(key,value)
获取value : getItem(key)
删除key: removeItem(key)
清除所有的key/value: clear(key/value)

另外，还支持点操作和[]
```
var storage = window.localStorage;
storage.key1 = "hello";
storage["key2"] = "world";
console.log(storage.key1);
console.log(storage["key2"]);
```
## storage事件
storage还提供了storage事件，当键值改变或者clear的时候，就可以触发storage事件，
如下面的代码就添加了一个storage事件改变的监听：
```
if(window.addEventListener){
    window.addEventListener('storage',func,false);
}else if(window.attachEvent){
    window.attachEvent('storage',func)
}
function func(e){
    //dosomething
}
```