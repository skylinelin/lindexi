---
title: "WPF 使用 SharpDx 渲染博客导航"
author: lindexi
date: 2019-3-6 17:58:47 +0800
CreateTime: 2019-3-6 16:57:24 +0800
categories: WPF
---

我写了很多在 WPF 使用 SharpDx 渲染的博客，但是有小伙伴吐槽说这些博客没有一个好看的顺序，我将所有的 SharpDx 的博客按照顺序放在下面

<!--more-->


<!-- csdn -->

因为 SharpDx 涉及很多底层的渲染知识，主要和 DirectX 相关，所以希望开始先看如何在 C# 用一个控制台创建一个 SharpDx 程序

- [C# 从零开始写 SharpDx 应用 控制台创建 Sharpdx 窗口](https://blog.lindexi.com/post/c-%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%86%99-sharpdx-%E5%BA%94%E7%94%A8-%E6%8E%A7%E5%88%B6%E5%8F%B0%E5%88%9B%E5%BB%BA-sharpdx-%E7%AA%97%E5%8F%A3 )

- [C# 从零开始写 SharpDx 应用 初始化dx修改颜色](https://blog.csdn.net/lindexi_gd/article/details/82114907 )

- [C# 从零开始写 SharpDx 应用 画三角](https://blog.lindexi.com/post/c-%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%86%99-sharpdx-%E5%BA%94%E7%94%A8-%E7%94%BB%E4%B8%89%E8%A7%92 )

其实 SharpDx 只是一个封装，在没有封装的时候可以直接使用 Direct2D1 在 WPF 画出界面

- [WPF 使用 Direct2D1 画图入门](https://lindexi.oschina.io/lindexi/post/WPF-%E4%BD%BF%E7%94%A8-Direct2D1-%E7%94%BB%E5%9B%BE%E5%85%A5%E9%97%A8.html )
- [WPF 使用 Direct2D1 画图 绘制基本图形](https://lindexi.oschina.io/lindexi/post/WPF-%E4%BD%BF%E7%94%A8-Direct2D1-%E7%94%BB%E5%9B%BE-%E7%BB%98%E5%88%B6%E5%9F%BA%E6%9C%AC%E5%9B%BE%E5%BD%A2.html )

但是使用Direct2D1写起来很难写，需要有一些封装，下面告诉大家如何在 WPF 使用 SharpDx 做界面

- [WPF 使用 SharpDX](https://blog.lindexi.com/post/wpf-%E4%BD%BF%E7%94%A8-sharpdx )
- [WPF 使用 SharpDX 在 D3DImage 显示](https://blog.lindexi.com/post/wpf-%E4%BD%BF%E7%94%A8-sharpdx-%E5%9C%A8-d3dimage-%E6%98%BE%E7%A4%BA )
- [WPF 使用封装的 SharpDx 控件](https://blog.lindexi.com/post/wpf-%E4%BD%BF%E7%94%A8%E5%B0%81%E8%A3%85%E7%9A%84-sharpdx-%E6%8E%A7%E4%BB%B6 )
- [WPF 使用 SharpDx 异步渲染](https://blog.lindexi.com/post/wpf-%E4%BD%BF%E7%94%A8-sharpdx-%E5%BC%82%E6%AD%A5%E6%B8%B2%E6%9F%93 )

更多请看 [SharpDX 系列](https://blog.lindexi.com/post/sharpdx.html )

如果想了解[渲染相关](https://blog.lindexi.com/post/%E6%B8%B2%E6%9F%93 )如 Win2d 和 WPF 渲染原理等请看 [渲染相关](https://blog.lindexi.com/post/%E6%B8%B2%E6%9F%93 )

以下是我收藏的一些博客

- [DirectX11 With Windows SDK--00 目录 - X_Jun - 博客园](https://www.cnblogs.com/X-Jun/p/9028764.html?tdsourcetag=s_pctim_aiomsg )

有任何建议和吐槽都欢迎通过邮件联系我，或加入[dotnet 职业技术学院](https://t.me/dotnet_campus) 交流

注意[dotnet 职业技术学院](https://t.me/dotnet_campus) 是一个电报群，加群需要一些技术

