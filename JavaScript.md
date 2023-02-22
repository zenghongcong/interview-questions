<!-- # 整理各种JavaScript面试题并按自己的理解进行实现 -->

### 1、打印的结果以及为什么顺序是这样？
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
#### 答案：
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

#### 2、实现一个深拷贝
```javascript
function deepClone(obj) {
    if (typeof obj !== 'object' || obj === null) return obj;
    const result = obj instanceof Array ? [] : obj instanceof Object ? {} : obj;
    Object.keys(obj).forEach((key) => {
        const val = obj[key];
        if (val instanceof Object || val instanceof Array) {
            result[key] = obj === val ? result : deepClone(val);
        } else {
            result[key] = val;
        }
    });
    return result;
}
```
1、基础类型直接赋值
2、引用类型进行递归
3、某个子属性引用了拷贝的对象本身，直接赋值result（极端情况）