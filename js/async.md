## 异步和同步的概念
异步：执行某个任务的同时可以执行其他任务
同步：就是连续的执行，第二个任务执行之前必须先等待第一个任务执行结束
## generater
异步编程的一种解决方案叫'协程'，意思是多个线程互相协作完成异步任务。Gnerator函数是
协程在ES6的实现。
定义：在普通函数前面加`*`就可以生成generator函数，特点是交出函数的执行权
（即暂停执行）
```
function *gen(x){
    var y=yield x +2;
    return y;
}
var g=gen(1);
g.next()//{value: 3, done: false}
g.next()//{value: undefined, done: true}
```
generater函数不同于普通函数，它的返回值是个内部指针（遍历器）g，调用g的next方法
会移动到内部指针（即执行异步任务的第一段），指向第一个遇到的 yield 语句，上例是
执行到 x + 2 为止。
换言之，next 方法的作用是分阶段执行 Generator 函数。每次调用 next 方法，会返回一
个对象，表示当前阶段的信息（ value 属性和 done 属性）。value 属性是 yield 语句后
面表达式的值，表示当前阶段的值；done 属性是一个布尔值，表示 Generator 函数是否执
行完毕，即是否还有下一个阶段。
## async函数用法
async函数返回一个Promise对象，可以用then方法添加回调函数。当函数执行的时候一旦遇到
`await`就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。
```
async function name([param]){}
```
调用异步函数时会返回一个promise对象，当这个异步函数返回一个值时，promise的resolve方法将会处理这个
返回值；当异步函数抛出的是异常或者非法值时，promise的reject方法将处理这个异常值

异步函数可能会包括`await`表达式，这将会使异步函数暂停执行并等待promise解析传值后，继续
执行异步函数的返回解析值。
例子：
```
function resolveAfter2Seconds(x){
    return new Promise(resolve=>{
        setTimeout(()=>{
            resolve(x)
            },2000)
        })
}
async function add1(x){
    var a=resolveAfter2Seconds(20)
    var b=resolveAfter2Seconds(30)
    return x + await a + await b
}
add1(20).then(r=>{
    console.log(r)//2s后输出70
    })
```
**注意点** ：await命令后面的Promise对象运行的结果可能是rejected，所以最好把await
命令放在try...catch代码中。
```
async function myFunction(){
    try{
        await somethingThatReturnsPromise();
        }catch(err){
           console.log(err)
        }
}
```

## 相对于Promise写法的优势
* 简洁
不需要写很多.then，不需要写匿名函数处理Promise的resolve值，也不需要定义多余的
data变量，还避免了嵌套代码。这些小的优点会迅速累计起来，这在之后的代码示例中会
更加明显。
实际案例：
```
(async function(){
    try{
         const res1=await axios.post('/submit');
         //dosomething
        }catch(err){
            console.log(err)
        }finally{
            console.log('go next');
        }
    try{
        const res2=await axios.post('/submit')
        //dosomething
        }
        catch(err){
            console.log(err)
            }
            finally{
                console.log('done');
            }
    })()
```