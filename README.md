# Interview
### 箭头函数与普通函数的区别
1、箭头函数是匿名函数，不能作为构造函数，不能使用new。<br>
2、箭头函数不绑定arguments，取而代之用rest参数...解决。<br>
3、箭头函数不绑定this，会捕获其所在的上下文的this值，作为自己的this值。<br>
4、箭头函数通过call()或apply()方法调用一个函数时，只传入了一个参数，对 this 并没有影响。<br>
5、箭头函数没有原型属性。<br>
6、箭头函数不能当做Generator函数,不能使用yield关键字
### 函数式编程
函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！
### 第一题：写 React / Vue 项目时为什么要在组件中写 key，其作用是什么？
  > key的作用是为了在diff算法执行时更快的找到对应的节点，提高diff速度。

### 第二题：['1', '2', '3'].map(parseInt) what & why ?
  > [1, NaN, NaN]

### 第三题: 什么是防抖和节流？有什么区别？如何实现？
  > 区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于wait，防抖的情况下只会调用一次，而节流的情况会每隔一定时间（参数wait）调用函数。<br>
  1. **防抖**
  > 触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间
  * __思路__
  > 每次触发事件时都取消之前的延时调用方法
  ```JavaScript
  function debounce(fn) {
    let timeout = null; // 创建一个标记用来存放定时器的返回值
    return function () {
      // 每当用户输入的时候把前一个 setTimeoutclear掉 
      clearTimeout(timeout);
      // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 
      // 间隔内如果还有字符输入的话，就不会执行fn函数
      timeout = setTimeout(() => {
        fn.apply(this, arguments);
      }, 500);
    };
  }
  function sayHi() {
    console.log('防抖成功');
  }

  var inp = document.getElementById('inp');
  inp.addEventListener('input', debounce(sayHi)); // 防抖
  ```
  2. **节流**
  > 高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率
  * __思路__
  > 每次触发事件时都判断当前是否有等待执行的延时函数
  ```JavaScript
  function throttle(fn) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
      if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
      canRun = false; // 立即设置为false
      setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
        fn.apply(this, arguments);
        // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环/// 了。当定时器没有执行的时候标记永远是false，在开头被return掉
        canRun = true;
      }, 500);
    };
  }
  function sayHi(e) {
    console.log(e.target.innerWidth, e.target.innerHeight);
  }
  window.addEventListener('resize', throttle(sayHi));
  ```
### 第四题：介绍下 Set、Map、WeakSet 和 WeakMap 的区别？
  * **Set**
  > 1. 成员不能重复。
  > 2. 只有健值，没有健名，有点类似数组。
  > 3. 可以遍历，方法有add, delete,has。
  * **weakSet**
  > 1. 成员都是对象
  > 2. 成员都是弱引用，随时可以消失。可以用来保存DOM节点，不容易造成内存泄漏
  > 3. 不能遍历，方法有add, delete,has

  * **Map**
  > 1. 本质上是健值对的集合，类似集合
  > 2. 可以遍历，方法很多，可以干跟各种数据格式转换
  * **weakMap**
  > 1. 直接受对象作为健名（null除外），不接受其他类型的值作为健名
  > 2. 健名所指向的对象，不计入垃圾回收机制
  > 3. 不能遍历，方法同get,set,has,delete
### 第五题：介绍下深度优先遍历和广度优先遍历，如何实现？
  TODO
### 第六题：使用css实现一个持续的动画效果
  * __CSS动画__
  ```css
  .box {
    width: 200px;
    height: 100px;
    background-color: #00ffff;
    position: relative;
    animation: myFrame 5s infinite;
    -webkit-animation: myFrame 5s infinite; /* Safari and Chrome */
  }
  /* 动画属性 */
  @keyframes myFrame {
    from {
      top: 0px;
    }
    to {
      top: 200px;
    }
  }
  @-webkit-keyframes myFrame { /* Safari and Chrome */
    from {
      top: 0px;
    }
    to {
      top: 200px;
    }
  }
  ```
  * __requestAnimationFrame动画__
  ```JavaScript
  var e = document.getElementById('e');
  var flag = true;
  var left = 0;
  setInterval(() => {
    left == 0 ? flag = true : left == 100 ? flag = false : '';
    flag ? e.style.left = ` ${left++}px` : e.style.left = ` ${left--}px`;
  }, 1000 / 60)
  ```
### 第七题: 右边宽度固定，左边自适应
  <details>
  <summary>第一种</summary>

  > css
  ```css
  body {
    display: flex;
  }
  .left {
    height: 100vh;
    background-color: #1122ff;
    flex: 1;
  }
  .right {
    width: 200px;
    height: 100vh;
    background-color: #ccddaa;
  }
  ```
  > html
  ```html
  <body>
    <div class="left"></div>
    <div class="right"></div>
  </body>
  ```
  </details>
  <details>
  <summary>第二种</summary>

  > css
  ```css
  .left {
    float: right;
    background-color: #66ee33;
    width: 200px;
    height: 100vh;
  }
  .right {
    margin-right: 200px;
    background-color: #ffee11;
    height: 100vh;
  }
  ```
  > html
  ```html
  <body>
    <div class="left"></div>
    <div class="right"></div>
  </body>
  ```
  </details>

### 第八题: 水平垂直居中
  <details>
  <summary>第一种 transform: translate(-50%, -50%) </summary>

  > css
  ```css
  .container {
    position: relative;
    width: 100%;
    height: 100vh;
  }
  .box {
    position: absolute;
    width: 300px;
    height: 200px;
    background-color: #888;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  ```
  ```html
  <div class="container">
    <div class="box">水平垂直居中</div>
  </div>
  ```
  </details>
  <details>
  <summary>第二种 margin: -height/2 0 0 -width/2 </summary>

  > css
  ```css
  .container {
    position: relative;
    width: 100%;
    height: 100vh;
  }
  .box {
    position: absolute;
    width: 300px;
    height: 200px;
    background-color: #888;
    top: 50%;
    left: 50%;
    margin: -100px 0 0 -150px;
  }
  ```
  > html
  ```html
  <div class="container">
    <div class="box">水平垂直居中</div>
  </div>
  ```
  </details>
  <details>
  <summary>第三种 margin: auto</summary>

  > css
  ```css
  .container {
    position: relative;
    width: 100%;
    height: 100vh;
  }
  .box {
    position: absolute;
    width: 300px;
    height: 200px;
    background-color: #888;
    margin: auto;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }
  ```
  > html
  ```html
  <div class="container">
    <div class="box">水平垂直居中</div>
  </div>
  ```
  </details>
  <details>
  <summary>第三种 display: flex </summary>

  > css
  ```css
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
  }
  .box {
    width: 300px;
    height: 200px;
    background-color: #888;
  }
  ```
  > html
  ```html
  <div class="container">
    <div class="box">水平垂直居中</div>
  </div>
  ```
  </details>

### 第九题 四种定位的区别
  > * `static` 是默认值
  > * `relative` 相对定位 相对于自身原有位置进行偏移，仍处于标准文档流中
  > * `absolute` 绝对定位 相对于最近的已定位的祖先元素, 有已定位(指position不是static的元素)祖先元素, 以最近的祖先元素为参考标准。如果无已定位祖先元素, 以body元素为偏移参照基准, 完全脱离了标准文档流。
  > * `fixed` 固定定位的元素会相对于视窗来定位,这意味着即便页面滚动，它还是会停留在相同的位置。一个固定定位元素不会保留它原本在页面应有的空隙。

### 第十题 let与var的区别
  > `let` 为 `ES6` 新添加申明变量的命令，它类似于 `var`，但是有以下不同：
  > * `var` 声明的变量，其作用域为该语句所在的函数内，且存在变量提升现象
  > * `let` 声明的变量，其作用域为该语句所在的代码块内，不存在变量提升
  > * `let` 不允许重复声明.

### 第十一题 Set 和 Map 数据结构
  > * `ES6` 提供了新的数据结构 `Set` 它类似于数组，但是成员的值都是唯一的，没有重复的值。
  > * `ES6` 提供了 `Map` 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，`Object` 结构提供了“字符串—值”的对应，`Map` 结构提供了“值—值”的对应，是一种更完善的 `Hash` 结构实现。

### 第十二题 重排和重绘
  > * 部分渲染树（或者整个渲染树）需要重新分析并且节点尺寸需要重新计算。这被称为重排。注意这里至少会有一次重排-初始化页面布局。
  > * 由于节点的几何属性发生改变或者由于样式发生改变，例如改变元素背景色时，屏幕上的部分内容需要更新。这样的更新被称为重绘。

### 第十三题 什么情况会触发重排和重绘？
  > * 添加、删除、更新 `DOM` 节点
  > * 通过 `display: none` 隐藏一个 `DOM` 节点-触发重排和重绘
  > * 通过 `visibility: hidden` 隐藏一个 `DOM` 节点-只触发重绘，因为没有几何变化
  > * 移动或者给页面中的 `DOM` 节点添加动画
  > * 添加一个样式表，调整样式属性
  > * 用户行为，例如调整窗口大小，改变字号，或者滚动。

### 第十四题 浏览器缓存
  > 浏览器缓存分为强缓存和协商缓存。当客户端请求某个资源时，获取缓存的流程如下：
  > * 先根据这个资源的一些 `http header` 判断它是否命中强缓存，如果命中，则直接从本地获取缓存资源，不会发请求到服务器；
  > * 当强缓存没有命中时，客户端会发送请求到服务器，服务器通过另一些`request header`验证这个资源是否命中协商缓存，称为`http`再验证，如果命中，服务器将请求返回，但不返回资源，而是告诉客户端直接从缓存中获取，客户端收到返回后就会从缓存中获取资源；
  > * 强缓存和协商缓存共同之处在于，如果命中缓存，服务器都不会返回资源；
  > * 区别是，强缓存不对发送请求到服务器，但协商缓存会。
  > * 当协商缓存也没命中时，服务器就会将资源发送回客户端。
  > * 当 `ctrl+f5` 强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存；
  > * 当 `f5` 刷新网页时，跳过强缓存，但是会检查协商缓存；

  * **强缓存**
  > * `Expires`（该字段是 `http1.0` 时的规范，值为一个绝对时间的 `GMT` 格式的时间字符串，代表缓存资源的过期时间）
  > * `Cache-Control:max-age`（该字段是 `http1.1` 的规范，强缓存利用其 `max-age` 值来判断缓存资源的最大生命周期，它的值单位为秒）
  * **协商缓存**
  > * `Last-Modified`（值为资源最后更新时间，随服务器`response`返回）
  > * `If-Modified-Since`（通过比较两个时间来判断资源在两次请求期间是否有过修改，如果没有修改，则命中协商缓存）
  > * `ETag`（表示资源内容的唯一标识，随服务器`respons`e返回）
  > * `If-None-Match`（服务器通过比较请求头部的`If-None-Match`与当前资源的`ETag`是否一致来判断资源是否在两次请求之间有过修改，如果没有修改，则命中协商缓存）



  





 