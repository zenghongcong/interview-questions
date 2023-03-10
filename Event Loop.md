### 打印的结果以及为什么顺序是这样？
```javascript
async function async1() {
    console.log('async1 start');
    await async2().then(res => console.log(res));
    console.log('async1 end');
}

async function async2() {
    console.log('async2');
    return 123;
}

console.log('script start');

setTimeout(function () {
    console.log('setTimeout');
}, 0);

async1();

new Promise(function (resolve) {
    console.log('promise1');
    resolve();
}).then(function () {
    console.log('promise2');
});

console.log('script end');
```
##### 答案：
```javascript
script start
async1 start
async2
promise1
script end
123
promise2
async1 end
setTimeout
```
##### 解题思路：
Event Loop（事件循环）可以简单概括为“先微后宏”，本题难点在于使用了**await**，**await**语句执行并返回了一个promise，**async1**函数与以下代码同等
```javascript
function async1() {
    console.log('async1 start');
    new Promise(resolve => {
        console.log('async2');
        resolve(123);
    }).then(res => {
        console.log(res)
    }).then(() => {
        console.log('async1 end');
    });
}
```
