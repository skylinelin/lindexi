---
title: "win10 uwp xaml 兼容多个版本条件编译"
author: lindexi
date: 2018-8-10 19:17:19 +0800
CreateTime: 2018-3-18 15:37:54 +0800
categories: Win10 UWP
---

如果开发的程序需要在多个版本运行，又需要使用最新版本的特性，那么请看本文。
本文告诉大家如何设置 xaml 的条件编译。

<!--more-->


<!-- csdn -->

如果只需要在 cs 判断版本，那么可以使用星期大神的代码，请看[UWP 判断系统版本](http://www.cnblogs.com/hupo376787/p/7766139.html )

但是如果是在 xaml ？ 我需要使用 16299 的功能，但是我需要让程序可以在 15063 运行，那么这时就需要 uwp xmal 条件编译。

使用的方法很简单，不过条件编译不是和 cs 代码使用 `#if` 的方式。

xaml 条件编译（Conditional XAML）就是 [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsApiContractPresent_System_String_System_UInt16_ ) 提供的标记。

因为这个标记稍微有些复杂，所以我先写代码给大家看

```csharp
  xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"

     <TextBlock Margin="200,200,0,0" contract5NotPresent:Text="以前的系统"
                   contract5Present:Text="最新的系统"></TextBlock>
```

![](http://image.acmx.xyz/65fb6078-c169-4ce3-cdd9-e35752d07be0%2F2018318154646.jpg)

因为xaml条件编译是在创意者更新 15063 支持的，所以需要先右击属性，设置最低版本为 15063，然后才可以编译

![](http://image.acmx.xyz/65fb6078-c169-4ce3-cdd9-e35752d07be0%2F2018318154958.jpg)

因为我的系统是 16299 所以运行就是显示最新的系统，如果是在 15063 的系统运行，因为我自己没运行，所以运行显示的我也不知道。

下面让我来告诉大家是如何写的。

首先在命名定义一个变量，这个变量是可以随意写的，我是直接抄微软代码，所以写为`contract5Present` ，他的值就是 http://schemas.microsoft.com/winfx/2006/xaml/presentation ，因为微软支持在最后面添加函数，所以加上函数`IsApiContractPresent ` 就是这样写

```csharp
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

那么需要来告诉大家 `IsApiContractPresent` 是做什么？

如果大家有打开 [UWP 判断系统版本](http://www.cnblogs.com/hupo376787/p/7766139.html )那么会发现判断系统的方法是通过最后的数字。

对应的数字

- 1：10240

- 2：10586

- 3：14393

- 4：15063

- 5：16299

如果在运行比数字低的版本，会返回true，例如 在运行 15063 的系统，可以看到下面的代码返回的值

 - IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = false

 - IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true

 - IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true

 - IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true

 - IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true

是的，如果运行在 16299 ，那么后面写  12345 都是返回true ，如果返回 true ，那么设置的属性才可以。如果返回false，那么在运行就不会有设置。

但是不能这样写代码

```csharp
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

如果需要判断在 16299 就设置这个属性，而在非 16299 就不设置这个属性，就需要使用`IsApiContractNotPresent` 对比一下，如果在 15063 的系统运行程序，那么下面代码就是这个值

 - IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = true
 - IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
 - IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
 - IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
 - IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

IsApiContractPresent 是在当前系统和低于当前系统返回 true ，IsApiContractNotPresent 是在非当前系统和不低于当前系统返回 true 

所以使用`IsApiContractPresent`和`IsApiContractNotPresent`可以设置当前使用的是那个版本

例如使用下面的代码

```csharp
    <RevealBorderBrush x:Key="KilqpdiHbmgvaz" TargetTheme="Light" Color="#08000000" FallbackColor="{ThemeResource SystemAccentColor}"/>
```

这时编译直接说不可以使用

![](http://image.acmx.xyz/65fb6078-c169-4ce3-cdd9-e35752d07be0%2F201831816240.jpg)

那么使用条件编译就可以让他编译通过

```csharp
    <contract5Present:RevealBorderBrush x:Key="KilqpdiHbmgvaz" TargetTheme="Light" Color="#08000000" FallbackColor="{ThemeResource SystemAccentColor}"/>
```

这时因为垃圾微软的 VisualStudio 的开发者不知道已经有这个，所以就告诉你错误，不要去理他，直接运行。

全部代码

```csharp
    <Page.Resources>

        <contract5Present:RevealBorderBrush x:Key="KilqpdiHbmgvaz" TargetTheme="Light" Color="#08000000" FallbackColor="{ThemeResource SystemAccentColor}" />

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid Margin="200,200,20,20"
              contract5Present:Background="{StaticResource KilqpdiHbmgvaz}"
              contract5NotPresent:Background="#FF565656" />

    </Grid>
```

实际上就是 contract5Present 支持设置属性或控件，可以使用 contract5Present 用新的系统的控件。

所以也可以使用下面的方法，例如在 16299 才有的 ColorPicker ，如果希望程序在 15063 使用，在以前的系统使用 ComboBox ，那么就可以使用下面的代码

```csharp
<contract5Present:ColorPicker />

<contract5NotPresent:ComboBox />
```

这样在新的系统就会使用 ColorPicker ，在以前的系统就会使用 ComboBox

如果在一个绑定一个使用了 contract5Present 的控件，那么在绑定的属性需要使用 contract5Present 不然微软的 VisualStudio 不然让你使用。

需要告诉大家，感觉说的 VisualStudio 在 Xaml 报告的错误，实际上这是Resharper的

如果觉得自己需要写的软件的版本比支持条件编译的版本还低，而且也不想写太多条件编译，请看[使用 Microsoft.UI.Xaml 解决 UWP 控件和对老版本 Windows 10 的兼容性问题 - walterlv](https://walterlv.github.io/post/getting-started-with-microsoft-ui-xaml.html )

参见

[Conditional XAML](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml )

[UWP 判断系统版本](http://www.cnblogs.com/hupo376787/p/7766139.html )

![](https://i.loli.net/2018/04/08/5ac9ffd774738.jpg)

