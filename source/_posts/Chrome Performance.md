# 学习使用Chrome性能分析工具（译）

> 原文地址：<https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/>

## 开始

在本教程中，你将学会如何使用性能分析工具分析页面上的性能瓶颈。

在隐身模式下打开Google Chrome。隐身模式确保Chrome在干净的状态下运行。例如，如果你安装了很多扩展，这些扩展可能会影响到性能分析的结果。

在隐身窗口中加载以下页面。这是本教程的Demo，页面显示了一堆上下移动的蓝色小方块。  

<https://googlechrome.github.io/devtools-samples/jank/>

接着按 F12 打开 DevTools。

![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2c2bc73a66?w=2316&h=1626&f=png&s=104543)

图1: Demo 在左侧，DevTools在右侧。

> 注意：为了保证更好的阅读体验，在后续的截图中，DelTools分到单独的窗口显示。

### 模拟移动设备的CPU

移动设备的CPU算力比台式机和笔记本电脑小得多。无论何时评测页面，都可以使用CPU调节来模拟页面在移动设备上的表现。

1. 在DevTools中，单击 **Performance** 选项卡。
2. 确保选中了 **Screenshots** 。
3. 单击 **Capture Settings**（设置按钮）。其中包含了捕获性能指标相关的设置。
4. 对于 **CPU**选项，选择 **2x slowdown**。DevTools会进行 CPU 节流，使其比平时慢2倍。  

![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2c2dad01e8?w=1238&h=816&f=svg&s=133109)

图2: CPU 节流，蓝色框区域

> 注意：在测试其他页面时，如果要确保它们在低端移动设备上工作良好，请将CPU节流设置为减速20倍。这个演示不能很好地使用20倍的减速，所以它只使用2倍的减速作为教学目的。

### 配置 Demo

很难为本网站的所有读者创建一致的运行时性能演示。本节允许你自定义演示，以确保你的体验与你在本教程中看到的屏幕截图和描述相对一致，而不管你的特定设置如何。

1. 继续单击 **Add 10**，直到蓝色方块移动明显比以前慢。在高端机器上，可能需要大约20次点击。
2. 单击 **Optimize**，蓝色方块应该移动得更快更流畅。
3. 单击 **Un-Optimize**，蓝色方块应该移动得更慢且更加卡顿

> 注意：如果你看不到优化版本和未优化版本之间的明显差异，请尝试单击 **Subtract 10** 几次，然后再试一次。如果你添加太多的蓝色方块，相当于把CPU都几乎占满了，就看不到优化和不优化版本的差异。

### 记录运行时的页面性能

当你运行优化版本时，蓝色方块移动得更快。为什么？两个版本都应该在相同的时间内，将每个方块移动相同的距离。在性能面板中录制，学习如何检测未优化版本中的性能瓶颈。

1. 在 DevTools 中，单击 **Record**（左上角灰色圆圈）。DevTools 会捕捉页面运行时的性能指标。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2c2d7daf42?w=1238&h=816&f=png&s=38986)

   图3：页面记录中

2. 等待几秒

3. 单击 **Stop**，DevTools 停止记录，分析数据，然后会将分析结果展示在性能面板中。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2c36973dea?w=1238&h=1376&f=png&s=78306)

   图4：分析的结果

   哇，这么多的数据。别慌，很快我们就知道具体的含义了。

## 分析结果

一旦你拿到了页面的性能分析数据，你会发现这个页面的性能到底有多差，并且找到导致页面性能差的原因。

### 分析帧率

衡量任何动画性能的主要指标就是帧率(FPS)。当动画以60 fps的速度运行时，用户会很爽。

1. 注意**FPS**图表。只要你看到一条红条，就意味着低帧率，进而影响用户体验。通常来说，绿色的柱条越高，代表帧率越高。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2ceb13e223?w=1238&h=820&f=svg&s=221251)

   图5：蓝框内的FPS图表

2. 在FPS图表下方，你可以看到CPU图表。CPU图表中的颜色与“性能”面板底部的 **Summary** 选项卡中的颜色相对应。CPU 图表充满颜色意味着CPU在记录过程中达到了最大负载。每当你看到CPU长时间达到最大负载时，这是进行优化的一个很好的提示。

   图6：蓝框内的 CPU 图表和 Summary（摘要栏）

3. 将鼠标悬停在 FPS、CPU 或 NET 图表上。DevTools 显示该页面在该时间点的屏幕截图。向左和向右移动鼠标以重放记录过程。这称为 scrubbing，它对于手动分析动画过程很有用。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2ce274e237?w=1297&h=856&f=png&s=137568)

   图7：查看页面在2000ms左右时的屏幕截图

4. 在 **Frames** 区域中，将鼠标悬停在其中一个绿色方块上。DevTools 向你显示该特定帧的 FPS。每帧可能远远低于60 FPS。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2d5f06bdce?w=1328&h=900&f=png&s=73569)

   图8：鼠标悬停在一帧上

   当然，在这个 DEMO 中，很明显这个页面的性能不是很好。但是在真实的场景中，我们不一定能一眼分辨出一个页面的性能好坏，所以使用这些工具来进行测量分析是很方便的。

### 查出性能瓶颈的根源

现在你已经测量并验证了页面动画表现不佳，接下来要回答的问题是：为什么？

1. 注意 **Summary** 选项卡，在未选择任何事件时，它呈现了浏览器在整个记录过程中把时间花在哪个部分。可以看到，页面的大部分时间都花在渲染上。所以现在的目标就是：减少浏览器花费在渲染工作上的时间。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2d15c5bc2d?w=1238&h=1108&f=svg&s=258770)

   图10：蓝框内的 **Summary** 选项卡

2. 展开 **Main** 区域，DevTools 向你展示了一段时间内主线程上活动图。x 轴代表着这段时间内的记录，每一个 Bar 都代表了一个事件。Bar 越宽，意味着该活动花费的时间更长。y轴表示调用堆栈，当你看到事件堆叠在一起时，这意味着上面的事件导致了下面的事件。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2d77abbe4f?w=1286&h=1064&f=svg&s=134575)

   图11：蓝框内的 **Main** 区域

3. 记录过程中有很多数据。在 **OverView** 面板（有 CPU, FPS, NET 图表的区域）上，用鼠标单击、按住、拖拽来放大单个 **Animation Frame Fired** 事件。此时 **Main** 和 **Summary** 中展示了选中的区间的相关信息。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2d87f50833?w=1238&h=962&f=png&s=61528)

   图12：放大单个 **Animation Frame Fired** 事件

   > 提示：你也可以通过单击 **Main** 中的某个事件后，通过鼠标的滚轮或者 **W,S,A,D** 键实现单个事件的缩放。

4. 注意在 **Animation Frame Fired** 事件右上角的红三角。只要你看到了红三角，这个事件就可能造成严重的问题。

   > 提示：每当 [`requestAnimationFrame()`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)  回调调用时, 都会触发 **Animation Frame Fired** 事件

5. 单击某个 **Animation Frame Fired** 事件, **Summary** 中会展示与该事件相关的信息. 注意 **reveal** 链接，单击后，DevTools 会将触发当前的  **Animation Frame Fired** 事件的事件高亮出来。同时注意 **app.js:94** 链接，单击后跳转到相应的源码。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2dca2c671d?w=1238&h=1270&f=png&s=73317)

   图13： 查看 **Animation Frame Fired** 事件的详细信息

   > 提示：选中一个事件之后，用左右方向键可以跳转到上/下一个事件

6. 在 **app.update** 事件下，有一堆紫色事件。稍微放大，看起来每个都可能有一个红色的三角形。现在单击其中一个紫色事件。DevTools 在 **Summary** 中提供了有关事件的详细信息。可以看到，有一个关于强制回流(forced reflows)的警告(也就是 Layout 的另一种说法)。

7. 在 **Summary** 中，单击 **Layout Forced** 下的 **app.js:70** 链接，DevTools 会跳转到引发强制回流的源代码。

   ![](https://user-gold-cdn.xitu.io/2019/8/3/16c56c2dc8882bbf?w=1238&h=794&f=png&s=46363)

   图14：导致强制回流的源代码

   > 注意：这行代码的问题在于：修改了蓝块样式之后，立刻读取蓝块 offsetTop 值。此时样式变更，而offsetTop 值是上一帧的值，浏览器为了保证读取 offsetTop 值的准确性，会先处理样式变更，然后重新布局以计算准确的 offsetTop 值，而重新布局（回流）的性能开销是很大的。参考：[Avoid_forced_synchronous_layouts](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing#avoid_forced_synchronous_layouts)

### 分析“优化版”的性能

使用刚刚学习的工作流和工具，单击演示中的优化以启用优化的代码，进行另一次性能记录，然后分析结果。从改进的帧率到 **Main** 中的活动图表中事件的减少，你可以看到应用程序的优化版本做的工作少得多，从而带来更好的性能。

#### 优化前后的性能分析图

![](https://user-gold-cdn.xitu.io/2019/8/3/16c56ca88bb4a71b?w=1068&h=870&f=png&s=141180)

#### 优化前后的代码对比

```js
app.update = function (timestamp) {
    for (var i = 0; i < app.count; i++) {
      var m = movers[i];
      if (!app.optimize) { // 1.普通版本
        var pos = m.classList.contains('down') ?
            m.offsetTop + distance : m.offsetTop - distance; // 读取offsetTop, 变更样式
        if (pos < 0) pos = 0;
        if (pos > maxHeight) pos = maxHeight;
        m.style.top = pos + 'px';
        if (m.offsetTop === 0) { // 样式变更后读取 offsetTop，导致回流
          m.classList.remove('up');
          m.classList.add('down');
        }
        if (m.offsetTop === maxHeight) { // 样式变更后读取 offsetTop，导致回流
          m.classList.remove('down');
          m.classList.add('up');
        }
      } else { // 2.优化版本
        var pos = parseInt(m.style.top.slice(0, m.style.top.indexOf('px')));
        m.classList.contains('down') ? pos += distance : pos -= distance; // 通过读取top，来获取原来蓝块的位置，避免读取 offsetTop 
        if (pos < 0) pos = 0;
        if (pos > maxHeight) pos = maxHeight;
        m.style.top = pos + 'px';
        if (pos === 0) { // 样式变更后用从样式 top 中读取到的位置信息进行判断，避免读取 offsetTop
          m.classList.remove('up');
          m.classList.add('down');
        }
        if (pos === maxHeight) {
          m.classList.remove('down');
          m.classList.add('up');
        }
      }
    }
    frame = window.requestAnimationFrame(app.update);
  }
```

> 注意：优化版本的代码虽然不会触发回流(Layout)，但依然会触发重绘(Paint)。一个更好的解决方案是使用只会触发**合成(Composite)**的属性，例如： transform 和 opacity。
>
> 参考： [Use transform and opacity changes for animations](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count#use_transform_and_opacity_changes_for_animations)

 

## 下一步

了解性能的基础是轨道模型(The RAIL model)。这个模型告诉你对你的用户来说最重要的性能指标。有关详细信息，请参见 [Measure Performance With The RAIL Model](https://developers.google.com/web/fundamentals/performance/rail) 。

为了让性能面板更舒适，熟能生巧。尝试分析自己的页面并分析结果。如果你对结果有任何疑问，[去Stack Overflow 提出关于 google-chrome-devtools 的问题](http://stackoverflow.com/questions/ask?tags=google-chrome-devtools)。如果可能，包括可复制页面的截图或链接。

要真正掌握运行时性能，你必须了解浏览器如何将HTML、CSS和JS转换为屏幕上的像，可以参考： [Rendering Performance Overview](https://developers.google.com/web/fundamentals/performance/rendering/). 这篇文章则更加深入：[The Anatomy Of A Frame](https://aerotwist.com/blog/the-anatomy-of-a-frame/) 

最后，有许多方法可以提高运行时性能。本教程将重点放在一个特定的动画瓶颈上，让你通过性能面板进行重点介绍，但这只是你可能遇到的众多瓶颈之一。**如何提升页面运行时的性能**还可以参考以下关于渲染性能的文章：

- [Optimizing JS Execution](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution)
- [Reduce The Scope And Complexity Of Style Calculations](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations)
- [Avoid Large, Complex Layouts And Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)
- [Simplify Paint Complexity And Reduce Paint Areas](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
- [Stick To Compositor-Only Properties And Manage Layer Count](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
- [Debounce Your Input Handlers](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)