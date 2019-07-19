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
  <summary>第一种 transform</summary>

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
 