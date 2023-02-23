### 实现简易版jQuery，支持链式调用为dom元素添加class

```javascript
$('p').addClass('red');
```

##### 答案：
```javascript
/**
 * 构造函数
 */
function JQuery() {}

// 原型对象
JQuery.prototype = {
    init(selector) {
        const nodes = document.querySelectorAll(selector);
        nodes.forEach((item, index) => {
            this[index] = item;
        });
        this.length = nodes.length;
    },
    addClass(cls) {
        for (let i = 0; i < this.length; i++) {
            const elem = this[i];

            // 使用clssName
            const { className } = elem;
            const classList = className.split(' ');
            classList.push(cls)
            elem.className = classList.join(' ');

            // 使用classList
            // elem.classList.add(cls);
        }
        return this;
    }
}

function $(selector) {
    const jQuery = new JQuery();
    jQuery.init(selector);
    return jQuery;
}
```

##### 解题思路：
1、使用**document.querySelectorAll**选中所有条件的dom元素
2、原型、原型链
3、每个原型对象上的方法执行完之后返回当前实例即可实现链式调用