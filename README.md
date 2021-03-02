## 2021年前端学习和沉淀，每天更新5题

> ### 2021-03-01

#### 1、0.1 + 0.2 === 0.3 吗？为什么？

<details>
<summary>答案</summary>

  JavaScirpt 使用 Number 类型来表示数字（整数或浮点数），遵循 IEEE 754 标准，通过 64 位来表示一个数字（1 + 11 + 52）

- 1 符号位，0 表示正数，1 表示负数 s
- 11 指数位（e）
- 52 尾数，小数部分（即有效数字）

最大安全数字：Number.MAX_SAFE_INTEGER = Math.pow(2, 53) - 1，转换成整数就是 16 位，所以 0.1 === 0.1，是因为通过 toPrecision(16) 去有效位之后，两者是相等的。

在两数相加时，会先转换成二进制，0.1 和 0.2 转换成二进制的时候尾数会发生无限循环，然后进行对阶运算，JS 引擎对二进制进行截断，所以造成精度丢失。

所以总结：精度丢失可能出现在进制转换和对阶运算中。

参考链接：

- [https://juejin.cn/post/6844903680362151950](https://)

</details>

***

#### 2、Number() 的存储空间是多大？如果后台发送了一个超过最大自己的数字怎么办

<details>
<summary>答案</summary>

<br/>

Math.pow(2, 53) ，53 为有效数字，会发生截断，等于 JS 能支持的最大数字。

</details>

***

#### 3、写代码：实现函数能够深度克隆基本类型

<details>

<summary>答案</summary>

浅克隆

```javascript
function shallowClone(obj) {
  let cloneObj = {};

  for (let i in obj) {
    cloneObj[i] = obj[i];
  }

  return cloneObj;
}
```

深克隆

- 考虑基础类型
- 引用类型
  - RegExp、Date、函数 不是 JSON 安全的
  - 会丢失 constructor，所有的构造函数都指向 Object
  - 破解循环引用

```javascript
function deepCopy(obj) {
  if (typeof obj === 'object') {
    var result = obj.constructor === Array ? [] : {};

    for (var i in obj) {
      result[i] = typeof obj[i] === 'object' ? deepCopy(obj[i]) : obj[i];
    }
  } else {
    var result = obj;
  }

  return result;
}
```

</details>

***

#### 4、new一个对象过程发生了什么

<details>

<summary>答案</summary>

<br/>

- 创建一个空对象
  ```javascript
  var obj = new Object();
  ```
- 设置原型链（当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象）
  ```javascript
  obj.__proto__= Func.prototype;
  ```
- 让Func中的this指向obj，并执行Func的函数体。（创建新的对象之后，将构造函数的作用域赋给新对象（因此this就指向了这个新对象））
  ```javascript
  var result =Func.call(obj);
  ```
- 判断Func的返回值类型：
  
    如果是值类型，返回obj。如果是引用类型，就返回这个引用类型的对象
  ```javascript
  if (typeof(result) == "object"){
    func=result;
  }
  else{
      func=obj;;
  }
  ```

</details>

***

#### 5、实现对象的map函数

<details>
<summary>答案</summary>

```javascript
Object.prototype.map = function(cb) {
  const obj = this
  const result = {}
  for(key in obj) {
    if (obj.hasOwnProperty(key)) {
      const item = cb(key, obj[key])
      result[key] = item
    }
  }
  return result
}

const test1 = {
  a:1,
  b:2,
  c:3
}

const test1Res = test1.map((key,val) => ++val) // { a: 2, b: 3, c:4 }
```

</details>

***

#### 6、****设置居中为什么推荐transform，而不是marginTop/Left****

- 答案

    对布局属性进行动画，浏览器需要为每一帧进行重绘并上传到 GPU 中

    对合成属性进行动画，浏览器会为元素创建一个独立的复合层，当元素内容没有发生改变，该层就不会被重绘，浏览器会通过重新复合来创建动画帧

    transform属于合成属性，对于合成属性进行动画，浏览器会创建一个合成层，使得动画元素在独立的层中进行动画。 通常情况下，浏览器会将一个层的内容现绘制一个位图中，再上传到GPU，只要该层的内容不发生改变，就不会进行重绘，浏览器会通过重新复合来形成一个新的帧。

    top/left属于布局属性，该属性的变化会导致重排（reflow/relayout），所谓重排即指对这些节点以及受这些节点影响的其它节点，进行CSS计算->布局->重绘过程，浏览器需要为整个层进行重绘并重新上传到 GPU，造成了极大的性能开销

    - ******参考链接****** [https://juejin.im/post/6844903753783443463]跟[https://blog.csdn.net/callmeCassie/article/details/89290945]

#### 7、Promise、async有什么区别

- 答案

#### 8、解释一下什么是面向对象编程

- 答案

    参考[https://zhuanlan.zhihu.com/p/75265007]

#### 9、call跟apply的区别，哪个性能更好一些？

- 答案

#### 10、this指向

- 答案
