## NPM
npm 是随同Nodejs一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：
* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
### 快速构建npm  `npm init`
利用`npm init -y`会快速创建一个package.json文件
### 要发布一个npm包需要在npm官网注册一个账号
注册成功后进入包的文件目录，登陆账号`npm login`
输入用户名和密码，登陆成功后可以输入查询命令`npm whoami`
### 上传npm包
cmd进到包文件目录，执行命令`npm publish`
如果自己的包名和别人重复会报错，请重命名后上传，注意包名不能有大写字母/空格/下划线
## [关于ComminJS、ES2015、AMD、CMD模块规范对比](https://www.hangge.com/blog/cache/detail_1686.html)
### CommonJS
模块规范： * 通过`require`来加载模块 
           * 通过  `exports`和`module.exports`来暴露模块中内容
NodeJS就是基于CommonJS
样例：
有一个模块hang.js 
```
exports.hello=function(){
    //...
}

//或者用module.exports
function hang(){
    ...
}
module.exports=hang;
```
main.js，调用上面模块
```
var hang=require('./hang')
```
### ES2015模块规范 
* `export`/ `export default`用于规定模块对外接口
* `import`用于输入其他模块提供的功能
hange.js模块
```
//输出模块
export default function(radius){
    return Math.PI *radius*radius
}
export function circumference(radius){
    return 2 * Math.PI * radius;
}
```
main.js
```
//引入模块,注意：对于 export default 指定模块的默认输出，import 语句不需要使用大括号。
import area,{circumference} from './hange'
```
## AMD
* AMD 通过异步加载模块。模块加载不影响后面语句的运行。所有依赖某些模块的语句均放置在回调函数中
* AMD 规范只定义了一个函数 define，通过 define 方法定义模块。该函数的描述如下：
   ```
   define(id?, dependencies?, factory)
    id：指定义中模块的名字（可选）。如果没有提供该参数，模块的名字应该默认为模块加载器请求的指定脚本的名字。如果提供了该参数，模块名必须是“顶级”的和绝对的（不允许相对名字）。
    dependencies：当前模块依赖的，已被模块定义的模块标识的数组字面量（可选）。
    factory：一个需要进行实例化的函数或者一个对象。
   ```
* AMD 规范允许输出模块兼容 CommonJS 规范
例子
```
define(["./cart", "./inventory"], function(cart, inventory) {
        //return an object to define the "my/shirt" module.
        return {
            color: "blue",
            size: "large",
            addToCart: function() {
                inventory.decrement(this);
                cart.add(this);
            }
        }
    }
);
```
## CMD
CMD 全称为 Common Module Definition，它是国内玉伯大神在开发 SeaJS 的时候提出来的。 
CMD 与 AMD 挺相近，二者区别如下： 
* 对于依赖的模块 CMD 是延迟执行，而 AMD 是提前执行（不过 RequireJS 从 2.0 开始，也改成可以延迟执行。 ）
* CMD 推崇依赖就近，AMD 推崇依赖前置
* AMD 的 api 默认是一个当多个用，CMD 严格的区分推崇职责单一，其每个 API 都简单纯粹。例如：AMD 里 require 分全局的和局部的。CMD 里面没有全局的 require，提供 seajs.use() 来实现模块系统的加载启动。
