## 原理
1. rem是一个半相对单位，它相对的是html或body元素的font-size值，例如有html{font-size:10px},则1rem=10px。
2. 当html元素的font-size是根据设备宽度自适应的，使用rem的的页面也就会自适应。
## 什么是dpr(设备像素比)
1. 物理像素（physical pixel）
一个物理像素是显示器（手机屏幕）上最小的物理显示物理单元，在操作系统的调度下，每一个设备都有自己的颜色值和亮度值。

2. 设备独立像素（density-independent pixel）
设备独立像素（也叫密度无关像素），可以认为是计算机坐标系统中得一个点，这个点代表一个可以由程序使用的虚拟像素（比如：css像素），然后由相关系统转换为物理像素。

3. 设备像素比（device pixel ratio）
设备像素比(简称dpr)定义了物理像素和设备独立像素的对应关系
设备像素比＝物理像素／设备独立像素 （在x方向或者y方向）
## 视觉稿
在前端开发之前，视觉MM会给我们一个psd文件，称之为视觉稿。

对于移动端开发而言，为了做到页面高清的效果，视觉稿的规范往往会遵循以下两点：

1. 首先，选取一款手机的屏幕宽高作为基准(以前是iphone4的320×480，现在更多的是iphone6的375×667)。
2. 对于retina屏幕(如: dpr=2)，为了达到高清效果，视觉稿的画布大小会是基准的2倍，也就是说像素点个数是原来的4倍（对iphone6而言：原先的375×667，就会变成750×1334）。

## 如何获取设备的dpr
通过`window.devicePixelRatio`获取
## 普通屏幕和retina屏幕
retina屏幕dpr=2
普通屏幕 dpr=1
## 可伸缩布局方案-手淘插件
```
(function flexible(window,document){
  var docE1=document.documentElement
  var dpr=window.devicePixelRatio||1

  //adjust body font size
  function setBodyFontSize(){
    if(document.body){
      document.body.style.fontSize=(12*dpr)+'px'
    }else{
      document.addEventListener('DOMContentLoaded',setBodyFontSize)//当初始HTML文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发
    }
  }
  setBodyFontSize();

  //set 1rem=viewWidth/10
  function setRemUnit(){
      var rem=docE1.clientWidth/10
      docE1.style.fontSize=rem+'px'
  }
  setRemUnit()

  //reset rem unit on page resize
  window.addEventListener('resize',setRemUnit)
  window.addEventListener('pageshow',function(e){
    if(e.persisted){
      setRemUnit()
    }
  })
  //delet 0.5px supports
  if(dpr>=2){
    var fakeBody=document.createElement('body')
    var testElemet=document.createElement('div')
    testElemet.style.border='.5px solid transparent'
    fakeBody.appendChild(testElemet)
    docE1.appendChild(fakeBody)
    if(testElemet.offsetHeight===1){
      docE1.classList.add('hairlines')
    }
    docE1.removeChild(fakeBody)
  }

  })(window,document)
```
## 多屏幕适配布局问题
基于rem原理我们要做的就是：** 针对不同手机屏幕尺寸和dpr动态的改变节点html的font-size大小（基准值） **
`rem=document.documentElement.clientWidth*dpr/10`
范例
```
var dpr,rem,scale;
var docE1=document.documentElement;
var fontE1=document.createElement('style');
var metaE1=document.querySelector("meta[name='viewport']");

dpr=window.devicePixelRatio||1;
rem=docE1.clientWidth*dpr/10;
scale=1/dpr;

//设置viewport，进行缩放，达到高清效果
metaE1.setAttribute('content','width='+dpr*docE1.clientWidth+',initial-scale='+scale+',maximum-scale='+scale+',minimum-scale='+scale+',user-scalable=no');

//设置data-dpi属性，留作css-hack使用
docE1.setAttribute('data-dpr',dpr);

//动态写入样式
docE1.firstElementChild.appendChild(fontE1);
fontE1.innerHTML='html{font-size:'+rem+'px!important}';

//给js调用，某一dpr下rem和dpr之间的转换函数
window.rem2px=function(v){
  v=parseFlot(v)
  return v/rem;
}
window.px2rem=function(v){
  v=parseFlot(v)
  return v/rem
}
```
## 字体不用rem,处理字体代码
```
//定义一个mixin 根据不同的dpr将px转化成相应的dpr的px值
/*
  @params $name 是css属性 比如width,top,font-size等
  @params $px 像素值
*/
@mixin px2px($name,$px){
    #{$name}:round($px/2)*1px;
    [data-dpr='2']&{
      #{$name}:$px*1px;
    }
    // for mx3
    [data-dpr="2.5"] & {
        #{$name}: round($px * 2.5 / 2) * 1px;
    }
    // for 小米note
    [data-dpr="2.75"] & {
        #{$name}: round($px * 2.75 / 2) * 1px;
    }
    [data-dpr="3"] & {
        #{$name}: round($px / 2 * 3) * 1px
    }
    // for 三星note4
    [data-dpr="4"] & {
        #{$name}: $px * 2px;
    }
}
```
