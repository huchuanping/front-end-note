# react三个js库
核心库`react.js`
`react-dom.js`提供与DOM相关的功能
`Browser.js`将JXS语法转化为Javascript语法
# ReactDOM.render()
是react的基本语法，用于将模板转为HTML语言，并插入指定的DOM节点
```
ReactDOM.render(
    <h1>Hello,world!</h1>,
    document.getElementById('example')
)
```
上面代码功能将h1标题插入到example节点
# JSX语法
html直接写在js中，不加任何引号，这就是jsx语法，允许html和js的混写
```
var names=['Jole','Grace','Emily'];
ReactDOM.render(
    <div>
    {
        names.map(function(name){
            return <div>Hello,{name}</div>
            })
    }
    </div>,
    document.getElementById('example')
)
```
JSX语法规则：遇到html标签（以`<`开头），就用HTML规则解析，遇到代码块（以`{`开头）,就用js
规则解析
JSX允许直接在HTML模板插入js变量，如果这个变量是个数组，会展开这个数组所有成员
```
var arr=[
<div>111</div>
<div>222</div>
];
ReactDOM.render(
<div>{arr}</div>,
document.getElementById('example')
)//页面上显示111,222
```
# 组件
将代码封装成组件，组件可以插入到html标签中
React.creatClass方法就用于生成一个组件类 
```
var HelloMessage=React.createClass({
    render:function(){
        return <h1>Hello {this.props.name}</h1>
    }
});
ReactDOM.render(
    <HelloMessage name="Grace" />,
    document.getElementById('example')
)
```
所有组件类必须有自己的`render`方法，用于输出组件
注意：组件类的第一个字母必须大写，否则会报错。组件类只能包含一个顶层标签，如上面已经有
h1标签了，不可以再写其他标签了，否则会报错。
组件与html标签完全一致，可以任意加入属性，属性可以通过`this.props`对象上获取
# this.props.children 
this.props.children属性表示组件的所有子节点 
```
var NotesList=React.createClass({
    render:function(){
        return(
            <ol>{
                React.Children.map(this.props.children,function(child){
                    return <li>{child}</li>
                    })
                }</ol>
        )
    }
})
ReactDOM.render(
<NotesList>
    <span>hello</span>
    <span>world</span>
</NotesList>,
document.getElementById('example')
)
```
`React.Children`用于处理`this.props.children`。`React.Children.map()`遍历子节点
# PropTypes
用于验证组件实例的属性是否符合要求
