![](http://p0.qhimg.com/t01fd2bba2aeda50f62.png)

利用伪元素、关键帧动画，你将具有强大的创造力，本文就是一个例子。本例中，利用两者，就可以构建一个加载动画，无需任何JS代码和图片。

### **你会问“为什么”**

首先，这是一篇关于CSS、伪元素、关键帧动画以及利用这些工具可实现哪些效果的文章。我不认为每个Web App都需要一个加载/启动界面，也不准备在本文中说服你相信这个。

我最近的一个[项目](https://healthy.io)中，在它加载好可用之前，第一步要做的是加载一段视频和几张图片。我不想立即显示内容，因为用户可能很快就要向下滚动界面，（页面未加载完）而不能享受完整的体验。我确实想保证一切加载完后，他们可以停留的时间足够长。

这就是我为什么决定构建这样一个尽可能快速显示出来的动画加载界面，直到其余的所有内容都准备完毕。为了实现它，我们只使用了HTML和CSS，没有使用任何额外的技术。

* * *


### **如何构建它**

你想要构建的加载界面因设计的不同，构建过程也会不一样。为了更具有普适性，我将以我的设计为例。

注意，本文假设你已熟悉伪元素、CSS animation属性及关键帧动画。如果你想复习一下后两者，推荐阅读[另一篇文章](https://www.smashingmagazine.com/2011/05/an-introduction-to-css3-keyframe-animations/)。还有[一篇文章](https://www.smashingmagazine.com/2011/07/learning-to-use-the-before-and-after-pseudo-elements-in-css/)，从中你可以全面了解伪元素。

好了，一切都准备好了吗？

#### **现在开始**

在决定仅用HTML和CSS构建它之前，我先做了一个动画版。

![](http://p0.qhimg.com/t01bb499bce759bf4a4.gif)

它可以给我们一个很好的参考。正如你看见的，这个动画包含4步。

1.  四个边框依次出现。

2.  红色、桔色、白色矩形滑入。

3.  三个矩形滑出。

4.  边框消失。

我们只需要构建第一步和第二步。使用`animation-direction: alternate;` 可以实现动画的反向执行， 从而完成第3步和第4步的构建。使用`animation-iteration-count: infinite;`可以实现动画的不断重复。

让我们从以下基本的HTML开始：

```
<!doctype html>
<html>
  <head>
    <!-- <link rel="preload"> for CSS, JS, and font files  -->
 **<style type="text/css">
      /*
       *  All the CSS for the loader
       *  Minified and vendor prefixed
       */
    </style>**
  </head>
  <body>
    **<div class="loader">
      <!-- HTML for the loader -->
    </div>**
    <header />
    <main />
    <footer />
    <!-- Tags for CSS and JS files -->
  </body>
</html>

```

CSS嵌入在头部（`<head></head>`）及body标签打开后加载出来的HTML中。可能还有更好的方法去利用浏览器的渲染路径？如果有，可以在评论中告诉我。

#### **构建Logo本身**

![](http://p0.qhimg.com/t017fce512a0fe049c2.png)

没有直接分析最终版本，我们试着遵循逻辑步骤，以便开发者可以用来构建相似的动画。在我的大脑里，第一步是构建没有任何动画效果的Logo。

`div.logo` 代表最外层矩形（父亲），`div.{$color}`代表里面的每一个矩形。

```
<div class="logo">
  <div class="white"></div>
  <div class="orange"></div>
  <div class="red"></div>
</div>

```

红色矩形在桔色矩形的后面，而桔色矩形在白色矩形的后面。因为默认情况下，元素按最后一个到第一个的顺序叠在一块。每个元素都针对某一边绝对定位，将来会从这一边出现（如，红色矩形从`left`，桔色矩形从 `bottom`）。同时给它们适当的`height`或`width`。

用SCSS，我们写出下面的代码：

```
div.logo {
  width: 100px;
  height: 100px;
  border: 4px solid black;
  box-sizing: border-box;
  position: relative;
  background-color: white;
  & > div {
    position: absolute;
  }
  div.red {
    border-right: 4px solid black;
    top: 0;
    bottom: 0;
    left: 0;
    width: 27%;
    background-color: #EA5664;
  }
  /* Similar code for div.orange and div.white */
  div.orange{
    border-top: 4px solid black;
    bottom: 0;
    right: 0;
    width: 73%;
    height:50%;
    background-color: #0000ff;
  }

div.white{
    border-left: 4px solid black;
    top: 0;
    right: 0;
    width: 20%;
    height:50%;
    background-color: #fffff;
  }
}

```

这是运行结果：

[running code can not be loaded]

#### **边框动画**

跟着我呢吧？接下来开始有意思的部分。

CSS不允许按我们的想法直接动画操作`div.logo`的边框。所以，我们必须从矩形上移除边框，寻求不同的方法创建它，一种可以动画操作的方法。

或许我们可以将边框打散成一个个小块，让它们循序地显现？我们可以使用两个透明的伪元素来覆盖整个矩形。

每次可以渲染出矩形四条边中的两条。然后我们通过让伪元素的`width`和`height`从0%至100%依次动画显示出来，从而让每个边框单独显示出来。

让我们试下吧。首先创建一个静态的版本。`div.logo::before`绝对定位于 `div.logo`左顶角，将显示顶部边框和右边框。`div.logo::after`定位于右底部，显示底部和左边框。

`div.logo`的SCSS看起来是这样的：

```
div.logo {
  width: 100px;
  height: 100px;
  box-sizing: border-box;
  position: relative;
  background-color: white;

  &::before,
  &::after {
    z-index: 1;
    box-sizing: border-box;
    content: '';
    position: absolute;
    border: 4px solid transparent;
    width: 100%;
    height: 100%;
  }

  &::before {
    top: 0;
    left: 0;
    border-top-color: black;
    border-right-color: black;
  }

  &::after {
    bottom: 0;
    right: 0;
    border-bottom-color: red; // Red for demo purposes only
    border-left-color: red;
  }
}

```

结果是：

[running code can not be loaded]

接下来，让我们用第一个关键帧为`div.logo::before`添加动画。

作为初始状态，该伪元素的`width`和`height`均设置为0。我们使用关键帧让`width`“动起来”变成100%，接下来，让 `height`变成100%。

伴随着该转换，边框的颜色也在适当的时刻由透明变成黑色，这样顶部和右侧的边框就会按我们预期的方式动起来。

包含伪元素初始状态改变的代码，见下：

```
div.logo {
  &::before,
  &::after {
    /* ... */
    width: 0;
    height: 0;
    animation-timing-function: linear;
  }

&::before {
    /* ... */
    animation: border-before 1.5s infinite;
    animation-direction: alternate;
  }
}

@keyframes border-before {
  0% {
    width: 0;
    height: 0;
    border-top-color: black;
    border-right-color: transparent;
  }
  24.99% {
    border-right-color: transparent;
  }
  25% {
    height: 0;
    width: 100%;
    border-top-color: black;
    border-right-color: black;
  }
  50%,
  100% {
    width: 100%;
    height: 100%;
    border-top-color: black;
    border-right-color: black;
  }
}

```


针对`div.logo::after`，我们采用同样的步骤，不要忘记修改时间，调换`width`和`height`。现在我们有了完整的边框动画。

[running code can not be loaded.]

#### **制作矩形动画**

终于，开始制作矩形动画了。

主要的挑战是关键帧间的关联。我们需要安排好每个动画，考虑好所有的步骤。这样，整个动画就会连续呈现。接着动画就可以反向执行。

针对边框的动画，我们简单地为每个边框分配25%的时间。这次我们把矩形添加进来。经过一系列的尝试和试错，我们选择在1.5s内按照以下策略加载各个部分：

1.  **0 to 25%:** 顶部和右边的边框出现。
2.  **25 to 50%:** 底部和左边的边框出现。

3.  **50 to 65%:** 红色矩形出现。

4.  **65 to 80%:** 桔色边框出现。

5.  **75 to 90%:** 白色边框出现。

根据以上时间轴，我们现在写出以下关键帧，为红色矩形的不透明度和宽度增加动画效果。

```
div.logo {
  div.red {
    /* ... */
    width: 0;
    animation: red 1.5s infinite;
    animation-direction: alternate;
  }
}

@keyframes red {
  0%,
  50% {
    width: 0;
    opacity: 0;
  }
  50.01% {
    opacity: 1;
  }
  65%,
  100% {
    opacity: 1;
    width: 27%;
  }
}

```

我们为桔色、白色矩形重复同样的过程，最后获得我们想要的结果：

[running code can not be loaded.]
                
