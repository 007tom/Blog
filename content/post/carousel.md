+++
title = "Carousel"
date = 2020-04-17T01:21:00+08:00
draft = false
[menu.other]
  weight = 1001
  identifier = "carousel"
+++

## [大佬](https://segmentfault.com/u/stillsilly)太强了，教我撸了个轮播图 {#大佬-太强了-教我撸了个轮播图}


### 初衷 {#初衷}

1.  CSS 基础布局的学习和练习
2.  JavaScript 语法和逻辑的学习和练习
3.  学会封装组件


### 项目地址 {#项目地址}

[JSRUN](http://jsrun.net/iHfKp/edit) , [gitee](https://gitee.com/write%5Fa%5Fcarousel/carousel) , [Github](https://github.com/zhyd1997/Carousel)


## 整体思路 {#整体思路}

使用 **绝对定位** 方法使 4 个 `div` 元素叠在一起


## 这只是 CSS 部分 {#这只是-css-部分}


### `div` 部分 {#div-部分}

```css
// parent element
.carousel-wrapper {
    position: relative;
    // 设置宽高
    width: 300px;
    height: 150px;
}
// children element
.carousel-item {
    position: absolute;
    display: none;
    // 和 parent 宽高保持一致
    width: 100%;
    height: 100%;
}
// 显示当前 div
.carousel-wrapper .carousel-item_current {
    display: block;
}
```


### indicator 部分 {#indicator-部分}

-   让 indicator 靠近底部，显示在 `div` 上面

<!--listend-->

{{< highlight css "hl_lines=7" >}}
.indicator-list {
    display: block;
    position: absolute;
    width: 100%;
    height: 10px;
    bottom: 15px;
    z-index: 999;
}

.indicator-item {
    background-color: green;
    width: 10px;
    height: 2.5px;
    display: inline-block;
}

.indicator-item_current {
    background-color: blue;
}
{{< /highlight >}}

-   水平居中，去掉换行符导致的间距

<!--listend-->

```css
.indicator-list {
    text-align: center;
    font-size: 0;
}
```

-   增加间距

<!--listend-->

```css
.indicator-item {
    margin-left: 2px;
    margin-right: 2px;
}
```


### arrow 部分 {#arrow-部分}

使用行内元素 `span`

-   设置水平位置

<!--listend-->

```css
.arrow {
    background-color: black;
    position: absolute;
    display: inline-block;
    font-size: 0;
    width: 25px;
    height: 25px;
}

.arrow-left {
    left: 15px;
}

.arrow-right {
    right: 15px;
}
```

-   垂直居中

<!--listend-->

```css
.arrow {
    margin: auto;
    top: 0;
    bottom: 0;
}
```

-   形状改为圆形，设置不透明度

<!--listend-->

```css
.arrow {
    border-radius: 50%;
    background-color: rgba(0, 0, 0, 0.2);
}
```

-   改变 `:hover` 不透明度

<!--listend-->

```css
.arrow:hover {
    background-color: rgba(0, 0, 0, 0.4);
}
```

-   添加 `cursor`

<!--listend-->

```css
.indicator-item {
    cursor: pointer;
}

.arrow {
    cursor: pointer;
}
```


## 右箭头的 `onclick` 事件绑定 {#右箭头的-onclick-事件绑定}


### 选择元素 {#选择元素}

```js
var elArrowRight = document.getElementsByClassName('arrow-right')[0];
var elListCarouselItem = document.getElementsByClassName('carousel-item');
```

事件绑定是发生在 `element` 身上的，
`document.getElementsByClassName()` 返回值是 (array-like object)
类数组对象 `HTMLCollection` ，类名为 `arrow-right` 的元素是当前要绑定的元素，对象后面跟 `[0]` 即可。


### 从 `console.log()` 开始 {#从-console-dot-log-开始}

```js
var currentIndex = 0; // 注意变量作用域

elArrowRight.onclick = function handleClick () {
    var oldIndex = currentIndex;

    if (currentIndex >= 3) { // 点击最后一个 div 要切回第一个 div
      currentIndex = 0;
    } else {
      currentIndex = currentIndex + 1;
    }

    console.log('现在是第 ' + oldIndex + ' 图显示，应该改成第 ' + currentIndex + ' 图显示');
}
```


### 效果实现 {#效果实现}

`div` 的隐藏和显示已经用 `css` 实现了，逻辑实现 `console.log()` 描述的也很清楚，接下来接下来要做的就是具体效果，改变 `elListCarouselItem` 的 `carousel-item_current` 位置，即点击右箭头之后，该 `class` 在当前 `<div>` 中删除，添加至下一个 `<div>` 中，实现方法用
`classList` 即可：

```js
elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
elListCarouselItem[currentIndex].classList.add('carousel-item_current');
```

**左箭头的实现方法同理，不再赘述。**


### 优化 {#优化}


#### `getElementsByClassName()` **vs** `querySelector()` {#getelementsbyclassname-vs-queryselector}

遇事不决，stackoverflow

论坛给出的[回答](https://stackoverflow.com/questions/14377590/queryselector-and-queryselectorall-vs-getelementsbyclassname-and-getelementbyid)是：querySelector 适用范围更广，比如选择 `<li>` 元素.

So，优化后的代码如下：

```js
var elArrowRight = document.querySelector('.arrow-right');
var elListCarouselItem = document.querySelectorAll('.carousel-item');
```


#### `handleClick()` {#handleclick}

这里加上函数名多此一举，直接用匿名函数

```js
elArrowRight.onclick = function () {...};
```


## 指示器的事件委托 {#指示器的事件委托}

现在要实现的功能是，点击指示器，切换至对应 `div` ，可以使用循环给 `indicator-item` 绑定事件，我太懒，直接用了事件委托的方法，参考[这篇文档](https://javascript.info/event-delegation)。

用事件委托的方法，绑定的元素就该是 `indicator-list` ，这就是其优于第一种方法的地方。

```js
var elIndicatorList = document.querySelector('.indicator-list');
var elIndicatorItem = document.querySelectorAll('.indicator-item');
```

同样，先做逻辑，再实现功能


### `console.log()` 对应 `indicator-item` 数字 {#console-dot-log-对应-indicator-item-数字}

要获取对应数字，须在 `.html` 自定义 `data-*` 属性， 即：

```html
<span data-index="0" class="indicator-item indicator-item_current"></span>
<span data-index="1" class="indicator-item"></span>
<span data-index="2" class="indicator-item"></span>
<span data-index="3" class="indicator-item"></span>
```

然后在 `.js` 文件中通过 `dataset` 获取属性。这里，我们需要的数字 `index` ，而不是字符串，需要将字符串转化为数字，有 `parseInt()` 和 `Number()` 可供选择，[查了一下](https://stackoverflow.com/questions/4090518/what-is-the-difference-between-parseint-and-number) ,选了 `parseInt()` ：

```js
elIndicatorList.onclick = function(event) {
    var target = event.target;
    var targetIndex = parseInt(target.dataset.index, 10);

    console.log(targetIndex);
}
```

这样，就可以获得所点击 `indicator-item` 对应的数字了。现在有一个 `corner case` ，
`indicator-list` 是一个和父元素等宽的元素，如果点击 `indicator-item` 之外的部分会有 bug，处理方式是事件委托仅对包含 `indicator-item` 类的元素起作用，点击其他位置即返回：

{{< highlight js "hl_lines=4-6" >}}
elIndicatorList.onclick = function(event) {
    var target = event.target;

    if (!target.classList.contains('indicator-item') {
      return;
    }

    var targetIndex = parseInt(target.dataset.index, 10);

    console.log(targetIndex);
}
{{< /highlight >}}


### 功能实现 {#功能实现}

实现方式和左右箭头的一样，关键是怎么知道上个状态的 `index` ，当前 `index` 即
`targetIndex` ，这里先用 `currentIndex` ，函数执行之后，将 `targetIndex` 赋给 `currentIndex`
作为上个状态时的 `oldIndex` ，看能不能跑起来：

{{< highlight js "hl_lines=9-16" >}}
elIndicatorList.onclick = function(event) {
    var target = event.target;

    if (!target.classList.contains('indicator-item')) {
      return;
    }

    var targetIndex = parseInt(target.dataset.index, 10);
    var oldIndex = currentIndex; // 前面已经定义 currentIndex 初始为 0

    elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
    elListCarouselItem[targetIndex].classList.add('carousel-item_current');
    elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
    elIndicatorItem[targetIndex].classList.add('indicator-item_current');

    currentIndex = targetIndex;
};
{{< /highlight >}}


## 整体优化 {#整体优化}


### 箭头和指示器同步 {#箭头和指示器同步}

点击箭头或者指示器的时候，
`indicator-item` 并不会发生变化，只需加如下两行代码即可：

```js
elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
elIndicatorItme[currentIndex].classList.add('indicator-item_current');
```


### 魔法数字问题 {#魔法数字问题}

在箭头 `onclick` 事件绑定中，使用了数字 3 来表示最后一个 `div` 的索引，这个数字叫做魔法数字，可用 `LIST_LENGTH - 1` 表示：

```js
const LIST_LENGTH = elCarouselItem.length;
```


### 消除重复代码（ _高优先级_ ） {#消除重复代码-高优先级}

当前三个点击事件的代码如下：

```js
elArrowRight.onclick = function() {
    var oldIndex = currentIndex;

    if (currentIndex >= LIST_LENGTH) { // currentIndex >= 4 即为 0
      currentIndex = 0;
    } else {
      currentIndex = currentIndex + 1;
    }

    elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
    elListCarouselItem[currentIndex].classList.add('carousel-item_current');
    elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
    elIndicatorItem[currentIndex].classList.add('indicator-item_current');
};

elArrowLeft.onclick = function() {
    var oldIndex = currentIndex;

    if (currentIndex <= 0) {
      currentIndex = LIST_LENGTH - 1;
    } else {
      currentIndex = currentIndex - 1;
    }

    elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
    elListCarouselItem[currentIndex].classList.add('carousel-item_current');
    elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
    elIndicatorItem[currentIndex].classList.add('indicator-item_current');
};

elIndicatorList.onclick = function(event) {
    var target = event.target;

    if (!target.classList.contains('indicator-item')) {
      return;
    }

    var targetIndex = parseInt(target.dataset.index, 10);
    var oldIndex = currentIndex;

    elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
    elListCarouselItem[targetIndex].classList.add('carousel-item_current');
    elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
    elIndicatorItem[targetIndex].classList.add('indicator-item_current');

    currentIndex = targetIndex;
};
```

观察发现，有大量很多可复用代码，现在要把这些代码抽取出来


#### `changeCurrentClassName(oldIndex, currentIndex)` {#changecurrentclassname--oldindex-currentindex}

```js
function changeCurrentClassName(oldIndex, currentIndex) {
    elListCarouselItem[oldIndex].classList.remove('carousel-item_current');
    elListCarouselItem[currentIndex].classList.add('carousel-item_current');
    elIndicatorItem[oldIndex].classList.remove('indicator-item_current');
    elIndicatorItem[currentIndex].classList.add('indicator-item_current');
}
```

现在整体代码变成这样了：

{{< highlight js "hl_lines=10 21 33" >}}
elArrowRight.onclick = function() {
    var oldIndex = currentIndex;

    if (currentIndex >= LIST_LENGTH) {
      currentIndex = 0;
    } else {
      currentIndex = currentIndex + 1;
    }

    changeCurrentClassName(oldIndex, currentIndex);
};
elArrowLeft.onclick = function() {
    var oldIndex = currentIndex;

    if (currentIndex <= 0) {
      currentIndex = LIST_LENGTH - 1;
    } else {
      currentIndex = currentIndex - 1;
    }

    changeCurrentClassName(oldIndex, currentIndex);
};
elIndicatorList.onclick = function(event) {
    var target = event.target;

    if (!target.classList.contains('indicator-item')) {
      return;
    }

    var targetIndex = parseInt(target.dataset.index, 10);
    var oldIndex = currentIndex;

    changeCurrentClassName(oldIndex, targetIndex);

    currentIndex = targetIndex;
};
{{< /highlight >}}

这里的 index 也是可以抽取的，下一步就是处理 index


#### `changeCurrentIndex(targetIndex)` {#changecurrentindex--targetindex}

```js
function changeCurrentIndex(targetIndex) {
    if (targetIndex >= LIST_LENGTH) {
      currentIndex = 0;
    } else if (targetIndex < 0) {
      currentIndex = LIST_LENGTH - 1;
    } else {
      currentIndex = targetIndex;
    }
}
```

前面获取上个状态的索引是通过 `oldIndex` ，其实用 `querySelector()`
更简单：

{{< highlight js "hl_lines=23 25" >}}
elArrowRight.onclick = function() {
    var targetIndex = currentIndex + 1;

    changeCurrentIndex(targetIndex);
    changeCurrentClassName();
};

elArrowLeft.onclick = function() {
    var targetIndex = currentIndex - 1;

    changeCurrentIndex(targetIndex);
    changeCurrentClassName();
};

elIndicatorList.onclick = function(event) {
    var targetIndex = parseInt(target.dataset.index, 10);

    changeCurrentIndex(targetIndex);
    changeCurrentClassName();
};

function changeCurrentClassName() {
    document.querySelector('.carousel-item_current').classList.remove('carousel-item_current');
    elListCarouselItem[currentIndex].classList.add('carousel-item_current');
    document.querySelector('.indicator-item_current').classList.remove('indicator-item_current');
    elIndicatorItem[currentIndex].classList.add('indicator-item_current');
{{< /highlight >}}


#### `handleIndexChange(targetIndex)` {#handleindexchange--targetindex}

```js
function handleIndexChange(targetIndex) {
    changeCurrentIndex(targetIndex);
    changeCurrentClassName();
}
```

处理一下 `corner case` ：如果当前索引和目标索引相等，即返回

{{< highlight js "hl_lines=2-4" >}}
function handleIndexChange(targetIndex) {
    if (targetIndex === currentIndex) { // 注意类型比较
      return;
    }

    changeCurrentIndex(targetIndex);
    changeCurrentClassName();
}
{{< /highlight >}}

最终代码如下：

```js
elArrowRight.onclick = function() {
    var targetIndex = currentIndex + 1;
    handleIndexChange(targetIndex);
};

elArrowLeft.onclick = function() {
    var targetIndex = currentIndex - 1;
    handleIndexChange(targetIndex);
};

elIndicatorList.onclick = functin(event) {
    var target = event.target;
    if (!target.classList.contains('indicator-item')) {
      return;
    }

    var targetIndex = parseInt(target.dataset.index, 10);
    handleIndexChange(targetIndex);
};
```


### 添加图片 {#添加图片}

前面的操作都是基于不同背景色的 `<div>` ，现在可以将其替换为真实的图片，使用了图床外链，设置一下样式：

```css
.carousel-item-img {
    width: 100%;
    height: 100%;
}
```


### 优化 `indicator-item` 效果 {#优化-indicator-item-效果}

`::after::` 是 `css3` 为区分伪类和伪元素的写法，不兼容 IE8

```css
.indicator-item::after {
    content: '';
    position: absolute;
    top: -3px;
    bottom: -3px;
    left: 0;
    right: 0;
}
```

给 `indicator-item` 添加 `transform` 效果，优化视觉体验：

```css
.indicator-item:hover {
    transform: scale(1.3);
}

.indicator-item_current {
    background-color: blue;
    transform: scale(1.3);
}
```


## 代码规范 {#代码规范}

1.  `width` 在前， `height` 在后
2.  CSS `bem` 命名规范，eg: `.carousel-item_current` , `.indicator-item_current`
3.  JavaScript 的 `camelCase` 命名法
4.  提炼函数，消除重复代码
5.  去除魔法数字