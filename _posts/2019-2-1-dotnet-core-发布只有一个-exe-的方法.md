---
title: "dotnet core 发布只有一个 exe 的方法"
author: lindexi
date: 2019-2-1 11:1:14 +0800
CreateTime: 2019-2-1 10:6:38 +0800
categories: dotnet
---

在 dotnet core 发布的时候，会使用很多文件，这样发给小伙伴使用的时候不是很清真，本文告诉大家一个非官方的方法通过 warp 将多个文件打包为一个文件

<!--more-->



<!-- csdn -->

和之前相同的方式发布一个 dotnet core 程序，记得需要使用 `--self-contained` 发布

```csharp
dotnet publish -c Release --self-contained -r win-x86

```

这时可以在输出的文件夹 bin 的 `Release\netcoreapp2.1\win-x86\publish` 文件夹看到输出的文件，可以看到输出的文件很多，这时通过 Powershell 下载 warp 工具

```csharp
[Net.ServicePointManager]::SecurityProtocol = "tls12, tls11, tls" ; Invoke-WebRequest https://github.com/dgiagio/warp/releases/download/v0.3.0/windows-x64.warp-packer.exe -OutFile warp-packer.exe
```

当然这个下载方法有些诡异，同时国内的网速也不是很好，可以通过 [官网](https://github.com/dgiagio/warp/releases/download/v0.3.0/windows-x64.warp-packer.exe) 或 [csdn](https://download.csdn.net/download/lindexi_gd/10946976) 下载

下载之后将 warp-packer.exe 放在 Release\netcoreapp2.1\win-x86\publish 的上一级文件夹里面，就放在 Release\netcoreapp2.1\win-x86 文件夹

这样就可以通过下面的命令打包出一个 exe 包含里面的文件

```csharp
当前的命令行路径是 Release\netcoreapp2.1\win-x86

> .\warp-packer --arch windows-x64 --input_dir .\publish\ --exec 在publish文件夹里面运行的程序 --output 输出的.exe
```

如在 Release\netcoreapp2.1\win-x86 里面的可运行程序 exe 是 lindexi.exe 我可以通过下面的代码合并里面的文件为一个 exe 文件

```csharp
.\warp-packer --arch windows-x64 --input_dir .\publish\ --exec lindexi.exe --output lindexi.exe
```

<!-- ![](image/dotnet core 发布只有一个 exe 的方法/dotnet core 发布只有一个 exe 的方法0.png) -->

![](http://image.acmx.xyz/lindexi%2F201921104230270)

同时使用这个工具还有一个好处，就是对文件进行压缩

限制：

当前（2019年1月3日）只能发布 x64 的版本的程序，如 windows x64 和 linux x64 程序。

[dgiagio/warp: Create self-contained single binary applications](https://github.com/dgiagio/warp#windows-1 )

[Single exe self contained console app · Issue #13329 · dotnet/corefx](https://github.com/dotnet/corefx/issues/13329 )

