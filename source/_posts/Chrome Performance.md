# 学习使用Chrome性能分析工具（译）

> 原文地址：<https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/>

## 开始

在本教程中，你将学会如何在实时页面上打开DevTools，并使用性能分析工具分析页面上的性能瓶颈。

在隐身模式下打开Google Chrome。隐身模式确保Chrome在干净的状态下运行。例如，如果您安装了很多扩展，这些扩展可能会影响到性能分析的结果。

在隐身窗口中加载以下页面。这是本教程的Demo。页面显示了一串上下移动的蓝色小方块。  

<https://googlechrome.github.io/devtools-samples/jank/>

接着按 F12 打开 DevTools。

![](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/get-started.png)

图片 1: Demo 在左侧，DevTools在右侧。

> 注意：为了保证更好的阅读体验，在后续的截图中，DelTools分到单独的窗口显示。

### 模拟移动设备的CPU

移动设备的CPU算力比台式机和笔记本电脑小得多。无论何时评测页面，都可以使用CPU调节来模拟页面在移动设备上的表现。

1. 在DevTools中，单击 **Performance** 选项卡。

2. 确保选中了 **Screenshots** 。

3. 单击 **Capture Settings**![](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/capture-settings.png)。其中包含了捕获性能指标相关的设置。

4. 对于 **CPU **选项，选择 **2x slowdown**。DevTools会进行 CPU 节流，使其比平时慢2倍。  

![](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/throttling.svg)

图片 2: CPU 节流，蓝色框区域

> 注意：在测试其他页面时，如果要确保它们在低端移动设备上工作良好，请将CPU节流设置为减速20倍。这个演示不能很好地使用20倍的减速，所以它只使用2倍的减速作为教学目的。

