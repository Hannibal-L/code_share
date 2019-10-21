# **Promise**

所谓 Promise，就是一个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理。

Promise 对象有以下两个特点：

* 对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和 Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是「承诺」，表示其他手段无法改变。

* 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolved 和从 Pending 变为 Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

Promise 的缺点：

* 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。

* 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。

* 当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

```
var promise = new Promise(function(resolve, reject) {
    if (/* 异步操作成功 */){
        resolve(value); // 成功
    } else {
        reject(error); // 失败
    }
});

promise.then((value) => {
    // success 成功回调 resolve状态
}, (value) => {
    // failure 失败回调 reject状态
}).cache((err) => { // cache等同于promise.then(null, ()=>{})
    // ... 失败处理 reject状态
}).finally(() => {
    // ... 不管成功失败
});
```

```
// 所有全部完成执行回调
var p = Promise.all([p1, p2, p3])
```

> Promise.prototype.then

```
* p.then(func1, func2) 接受resolve和reject的信息
* p.then(func1) 接受resolve的信息
* p.then(null, func2) 只接受reject的信息
```

# Async-Await

    async-await是寄生于Promise的语法糖，方便解决Promise callback地狱问题。async await可以简写为await

规则：

* async表示这是一个async函数，await只能用在这个函数里面。
* async函数返回的是一个Promise对象。
* await后面跟着的应该是一个Async函数，返回Promise对象， 如果是Promise<T>，返回值相当于Promise.then((value)=>{})中的value。 (待确定)
* await表示在这里等待返回结果后， 再继续执行。 相当于Promise.then()。
* 用try cache包含await函数捕捉reject状态。
* Typescript 最终也会把 async/await 的方式编译成 js 中类似 yield/generator 的方式

> 语法

```
async callFunc():Promise<boolean> {
    try {
        let bool = await promiseFunc();
    } catch(e) { // 异常捕捉
        console.log(e)
    }
    console.log("call promiseFunc") // 先输出日志
}

promiseFunc() : Promise<boolean> {
    console.log("run promiseFunc") // 后输出日志
    let p = new Promise<boolean>((resolve, reject) => {
        if (true) {
            resolve(true)
        } else {
            resolve(false)
        }
        reject("unknow err")
    });
    return p;
}
```