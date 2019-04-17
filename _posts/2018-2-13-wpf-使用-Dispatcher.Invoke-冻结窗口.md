---
title: "wpf 使用 Dispatcher.Invoke 冻结窗口"
author: lindexi
date: 2018-2-13 17:23:3 +0800
CreateTime: 2018-2-13 17:23:3 +0800
categories: WPF 性能优化
---

如果使用`Dispatcher.Invoke`实际上会有一个坑，在执行`Dispatcher.Invoke`刚好拖动窗口就会出现窗口冻结，这时使用 Alt+Tab 可以解决。

<!--more-->


<div id="toc"></div>

<!-- csdn -->

<!-- 标签：wpf,性能优化 -->

这个问题是在我写[wpf DoEvents](https://lindexi.oschina.io/lindexi/post/wpf-DoEvents.html )发现的，因为`Dispatcher.Invoke`可以让界面刷新，但是在拖动窗口会让窗口冻结。

所以一个建议的方法是使用`Dispatcher.InvokeAsync` ，如果需要深入了解，请看我师傅的文章[深入了解 WPF Dispatcher 的工作原理](https://walterlv.github.io/post/dotnet/2017/09/26/dispatcher-invoke-async.html )

在所有使用`Dispatcher.Invoke`的代码都可以通过使用`await Dispatcher.InvokeAsync`去替换。

