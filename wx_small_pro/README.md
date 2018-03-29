## 简介
1. 通过微信开发者工具完成小程序的创建和代码编辑
2. 三个不可或缺的文件app.js,app.json,app.wxss 
3. app.js 用于调用框架提供的丰富的API 
4. app.json 是对整个小程序的全局配置，如页面组成，小程序窗口背景色标题等，该文件不可以添加任何注释
5. app.wxss 是整个小程序的公共样式表。
6. 小程序框架包括视图层和逻辑层
## 逻辑层
1. 注册小程序
  `App(object)`,该函数用来注册一个小程序，参数object指定小程序的生命周期函数
  object参数说明
  * onLaunch 生命周期函数，监听小程序初始化，当小程序初始化完成时会触发该函数，全局只触发一次
  * onShow 生命周期函数，监听小程序显示，当小程序启动或从后台进入前台显示，会触发该函数
  * onHide 生命周期函数，监听小程序隐藏，当小程序从前台进入后台会触发
  * onError 错误监听函数，当小程序发生脚本错误，或者api调用失败时会触发同时带上错误信息
  `getAppp()`，全局函数，用于获取小程序实例
  ```
  var app=getApp()
  console.log(app.globalData)
  ```
2. 注册页面
 `Page(object)`该函数用来注册一个页面，object指定页面初始数据、生命周期函数、事件处理函数
  object参数说明
  * data 页面初始数据
  * onLoad 生命周期函数，监听页面加载
  * onReady 生命周期函数，监听页面初次渲染完成
  * onShow  生命周期函数，监听页面显示
  * onHide 生命周期函数，监听页面隐藏
  * onUnload 生命周期函数，监听页面卸载
  * onPullDownRefresh 事件处理函数，监听用户下拉动作
  * onReachBottom 监听用户上拉触底事件的处理函数
  * onShareAppMessage 用户点击右上角转发
  * onPageScroll 页面滚动触发事件的处理函数
  * 开发者也可以添加任意的函数或者数据到object参数中，在页面函数中用this可以访问
## 视图层
### WXML
1. 数据绑定 {{}}
2. 列表渲染 wx:for="{{array}}"
3. 条件渲染 wx:if="{{condition}}"
4. 可以在模板中定义代码片段，然后在不同的地方调用，
   定义：`<template name='tempname'></template>`
   使用：`<template is='tempname' data="{{}}"></template>`
   is可以使用{{}}来动态决定具体需要渲染哪个模板
5. 事件绑定 bind/catch + 事件类型;
   冒泡事件：tap,longtap,touchstart,touchmove,touchcancel,touchend;
   除了上述事件其他自定义事件都是非冒泡事件;
   bind事件绑定不会阻止事件向上冒泡，catch事件可以阻止冒泡事件向上冒泡
6. 引用 `import`和`include`
   `import`可以在文件中使用目标文件定义的template
    ```
    <!-- index.wxml 引用目标文件item.wxml -->
    <import src="item.wxml"/>
    <template is="item" data="{{}}"></template>
    ```
    import有作用域的概念，即只会import目标文件中定
    义的template，而不会import目标文件import的template;
    `include`可以将目标文件除了`<template/>`的整个代码引入，相当于拷贝到include位置
### WXS(小程序一套脚本语言)
1. 页面渲染
```
<wxs module="ml">
    var msg="hello world33";
    module.exports.message=msg;
</wxs>
<view>{{ml.message}}</view>
```
2. WXS模块
   WXS代码可以编写在wxml文件中的`<wxs>`标签内，或以.wxs为后缀名的文件内。
   一个模块想要对外暴露其内部的私有变量与函数，只能通过`module.exports`实现;
   在.wxs内部引用其他wxs文件模块，可以使用`require`函数。
   `<wxs>`有两个属性`module`和`src`
   `module`属性是当前<wxs>模块的模块名;
    `src`属性用来引用其他的wxs文件模块;
3. 变量
   和javascript变量定义一致
4. 注释
 分为单行注释、多行注释、结尾注释
## WXSS
具有css大部分属性，也扩展了css的属性，特有属性有：
尺寸单位rpx和格式导入@import
## 组件
### 视图容器组件
`<view></view>`视图容器
`<scroll-view></scroll-view>`可滚动视图容器
`<swiper></swiper>`滑块视图容器
`<movable-area></movable-area>` 可移动的视图容器 必须设置宽高
`<cover-view></cover-view>>`内容覆盖容器
### 基础内容组件
内容组件 
图标 `<icon type="" size="" color=""></icon>`
文本 `<text selectable="是否可选" space=""></text>`
富文本 `<rich-text></rich-text>`
进度条 `<progress percent="" />`
### 表单组件
### 导航组件
### 媒体组件
### 地图组件
### 画布组件
## API
### 网络API
1. 发起请求 wx.request(object),类似ajax()
2. 上传、下载 wx.uploadFile(object) wx.downloadFile(object)
3.  Websocket wx.connectSocket(object)
### 媒体API
1. 图片 
* wx.chooseImage(object) 从本地相册选择图片或使用相机拍照
* wx.previewImage(object) 预览图片
* wx.getImageInfo(object)获取图片信息
* wx.saveImageToPhotosAlbum(object)保存图片到系统相册，需要用户授权
2. 录音
* wx.startRecord(object)
* wx.stopRecord(object)
3. 音频播放控制
4. 音乐播放控制
### 文件API
* wx.saveFile(object)
### 数据API
wx.setStorage()
wx.getStorage()
wx.clearStorage()
### 位置API
wx.getLocation()返回当前的地理位置、速度


