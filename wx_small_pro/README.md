## 简介
1. 通过微信开发者工具完成小程序的创建和代码编辑
2. 三个不可或缺的文件app.js,app.json,app.wxss 
3. app.js 用于调用框架提供的丰富的API 
4. app.json 是对整个小程序的全局配置，如页面组成，小程序窗口背景色标题等，该文件不可以添加任何注释
5. app.wxss 是整个小程序的公共样式表。
6. 小程序框架包括视图层和逻辑层
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
4. 
