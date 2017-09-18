## async function 声明定义了一个异步函数，它返回一个AsyncFunction对象
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