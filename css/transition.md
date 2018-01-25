# 前言
  页面效果是很重要的一环，好的页面效果，可以让用户感觉很舒服，进而能吸引更多的用户。  
  既然是页面效果，那么**动画**肯定不可或缺，那么这篇先说下 ***transition*** 这个 *css3* 属性。

# 初探 *transition* 
  >  ***transitions*** 提供了一种在更改 *CSS* 属性时控制动画速度的方法。其可以让属性变化成为一个持续一段时间的过程，而不是立即生效  的。（摘自[MDN][1]）
  
## 用法

  - 其写法（为了兼容，要加厂商前缀，这里不写咯）：
  ```
  transition: <property> <duration> <timing-function> <delay>
  ```
  <br/>

  - 也可以用逗号隔开，多写几个作用于不用的 **property**（属性）
  ```
  transition: <property> <duration> <timing-function> <delay>, <property> <duration> <timing-function> <delay>
  ```
## 各项作用

   @ | 值 |默认值| 作用 
   -- | -- | :--: | -- 
   **property** | none / all / [property](#property) | all | 作用于需要过渡的属性 
   **duration** | [time](#time) | 0 | 过渡所需时间 
   **timing-function** | linear / ease / ease-in / ease-out / ease-in-out / [cubic-bezier(n,n,n,n)](#bezier) | ease | 规定过渡效果的速度曲线 
   **delay** | time (同*duration*) | 0 | 延迟多久才开始过渡 
  <br/>

  1.  <b id="property">property</b>：可过渡的 CSS 属性，有些属性则不行。
  2.  <b id="time">time</b>：以秒（如`1s`）或毫秒（如`1000`）计。
  3.  <b id="bezier">cubic-bezier(x1,y1,x2,y2)</b>：[三阶贝塞尔曲线][3]。*linear* ，*ease* 等等都是由它计算出来的。**x轴** 的取值范围：[0,1] 区间内, **y轴** 可以任意值。

# 动手使用下

## 基本操作

  ```
  // css
  #box {
    width: 100px;
    height: 100px;
    background-color: aqua;
    transition: all 1s;
    -webkit-transition: all 1s;
    -moz-transition: all 1s;
    -ms-transition: all 1s;
    -o-transition: all 1s;
  }

  #box:hover {
    width: 500px;
    margin-top: 100px;
  }

  // html
  <div id="box"></div>
  ```
  * [在线demo][2]
  * `#box`的 *style* 里有`transition`，要触发它，则需要改变上面所说的 [property](#property) 。
  * 当鼠标移到 `div` 时，`width` 和 `marin-top` 发生了变化，触发了 `transition` 。
  * **all** ： 所有可过渡的属性都可以的意思。
  * **1s** ： 在第二项，即 `duration` 。过渡所需时间。

## 简单的类似图片无缝滑动

  ```
  // css
  * {
    margin: 0;
    padding: 0;
  }

  #box {
    margin: 0 auto;
    width: 100px;
    height: 100px;
    overflow: hidden;
    
  }

  #img_container {
    position: relative;
    width: 400px;
    transition: all .7s linear;
    -webkit-transition: all .7s linear;
    -moz-transition: all .7s linear;
    -ms-transition: all .7s linear;
    -o-transition: all .7s linear;
  }

  #img_container span {
    display: inline-block;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    background-color: yellow;
  }

  #img_container span:nth-of-type(even) {
    background-color: red;
  }

  ul {
    text-align: center;
  }

  li {
    padding: 10px;
    cursor: pointer;
    list-style: none;
    display: inline-block;
  }

// html
  <div id="box">
    <section id="img_container">
       <span>1</span><span>2</span><span>3</span><span>4</span>
    </section>
  </div>
  <ul>
    <li>图一</li>
    <li>图二</li>
    <li>图三</li>
    <li>图四</li>
  </ul>

  // js
  ~function () {
      let lis    = document.getElementsByTagName('li');
      let imgBox = document.getElementById('img_container');
      let width  = +document.querySelector('span').clientWidth;
      
      for (let j = 0;j < lis.length;j++) {
        
        lis[j].addEventListener('click',function() {
          imgBox.style.marginLeft = `-${ width * j }px`;
        });

      }

  }()
  ```

  * 直接看[在线demo][4]。
  * 这个 demo 是用 **js** 改变了 **property** ，从而触发 `transition`。
  
## 滚动到某个位置，动画才出现
  

  # 未完!




[1]: https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions
[2]: https://codepen.io/anon/pen/jYozwM
[3]: https://zh.wikipedia.org/wiki/%E8%B2%9D%E8%8C%B2%E6%9B%B2%E7%B7%9A#%E4%B8%89%E6%AC%A1%E6%96%B9%E8%B2%9D%E8%8C%B2%E6%9B%B2%E7%B7%9A
[4]: https://codepen.io/anon/pen/eywmEP