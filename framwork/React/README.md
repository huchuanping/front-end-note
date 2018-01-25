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
组件的属性可以接受任意值，字符串、对象、函数等。
用于验证组件实例的属性是否符合要求
```
var MyTitle=React.createClass({
    propTypes:{
        title:React.PropTypes.string.isRequired,
    },
    render:function(){
        return <h1>{this.props.title}</h1>;
    }
});
```
上面代码表示组件`MyTitle`有一个title属性，`PropTypes`告诉Reat，这个title属性是必须的，
而且它的值必须是字符串。
```
var data=123;
ReactDOM.render(
    <MyTitle title={data}/>,
    document.getElementById('example')
)
```
`getDefaultProps`用于设置组件属性的默认值
# 获取真实的DOM节点
组件并不是真实的DOM节点，是存在于内存之中的一种数据结构，只有当被插入到文档后才会变成真实的DOM
节点。所有的DOM变动都是在虚拟DOM上发生，然后再将实际变动的部分反应在真实DON上，被称之为DOMdiff，
提高网页的性能表现。
如果要从组件获取真实的DOM节点，就要用到`ref`属性。
```
var MyComponent=React.createClass(
    handleClick:function(){
        this.refs.myTextInput.focus();
    },
    render:function(){
        return (
            <div> 
                <input type="text" ref="myTextInput"/>
                <input type="button" value="Focus input" onClick={this.handleClick} />
            </div>
        )
    }
)

ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
)
```
上述代码解释：组件MyComponent的子节点有一个文本输入框，获取用户输入，此时必须获取真实的
DOM节点，虚拟DOM拿不到用户输入，因此给文本框增加`ref`属性，然后通过`this.refs.[refName]
`就会返回这个真实的DOM节点。
需要注意的是，由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入
文档以后，才能使用这个属性，否则会报错。上面代码中，通过为组件指定 Click 事件的回调函数，
确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName] 属性。
# this.state 
React将组件看成一个状态机，一开始有一个初始状态，当用户互动导致状态变化，从而出发重新
渲染UI
```
var LikeButton=React.createClass({
    getInitialState:function(){
        return {liked:false}
    },
    handleClick:function(event){
        this.setState({liked:!this.state.liked});
    },
    render:function(){
        var text=this.state.liked ? 'like' : 'haven\'t liked';
        return (
            <p onClick={this.handleClick}>
                You {text} this.Click to toggle.
            </p>
        )
    }
})
ReactDOM.render(
    <LikeButton/>,
    document.getElementById('example')
)
```
# 表单
用户在表单中填入的内容属于用户和组件的互动，通过`event.target.value`获取用户输入的值
```
var InputComponent=React.createClass({
    getInitialState:function(){
        return {value:'Hello'}
    ,
    handleChange:function(event){
        this,getState({value:event.target.value})
        },
    render:function(){
        var value=this.state.value;
        return (
            <div>
                <input type="text" value={value} onChange={this.handleChange}/>
                <p>{value}</p>
            </div>
        )
    }
});
ReactDOM.render(
    <InputComponent />,
    document.getElementById('example')
)
```
# 组件生命周期
组件生命周期分三个状态：
* Mounting 已插入真实DOM
* Updateing 正在被重新渲染
* Unmounting 已被移除真实DOM
每种状态有两个处理函数`Will`(在进入状态之前调用)，`did`在进入状态之后调用，三种状态共有5
5种处理函数:
* couponentWillMount() 进入状态1之前
* componentDidMount() 进入状态1之后
* componentWillUpdate(object nextProps,object nextState) 进入状态2之前
* componetDidUpdate(object nextProps,object nextState)进入状态2之后
* componentWillUnmount()进入状态3之前
```
var Hello=React.createClass({
    getInitialState:function(){
        return {
            opacity:1.0
        }
    },
    componentDidMount:function(){
        this.timer=setInterval(function(){
            var opacity=this.state.opacity;
            opacity-=.05;
            if(opacity<0.1){
                opacity=1.0 
            }
            this.setState({opacity:opacity})
            }.bind(this,100))
    },
    render:function(){
        return(<div style={{opacity:this.state.opacity}}>Hello {{this.props.name}}</div>);
    }
});
ReactDOM.render(
    <Hello name="world"/>,
    document.getElementById('example')
)
```
这里注意组件样式的写法style={{opacity:this.state.opacity}},这样写是因为React组件样式
是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象。
# Ajax
组件的数据来源通常是通过Ajax请求从服务器获取，可以试用`componentDidMount`方法设置Ajax
请求
```
var RepoList = React.createClass({
  getInitialState: function() {
    return {
      loading: true,
      error: null,
      data: null
    };
  },

  componentDidMount() {
    this.props.promise.then(
      value => this.setState({loading: false, data: value}),
      error => this.setState({loading: false, error: error}));
  },

  render: function() {
    if (this.state.loading) {
      return <span>Loading...</span>;
    }
    else if (this.state.error !== null) {
      return <span>Error: {this.state.error.message}</span>;
    }
    else {
      var repos = this.state.data.items;
      var repoList = repos.map(function (repo, index) {
        return (
          <li key={index}><a href={repo.html_url}>{repo.name}</a> ({repo.stargazers_count} stars) <br/> {repo.description}</li>
        );
      });
      return (
        <main>
          <h1>Most Popular JavaScript Projects in Github</h1>
          <ol>{repoList}</ol>
        </main>
      );
    }
  }
});

ReactDOM.render(
  <RepoList promise={$.getJSON('https://api.github.com/search/repositories?q=javascript&sort=stars')} />,
  document.getElementById('example')
);
```