---
title: "win10 uwp smms图床"
author: lindexi
date: 2018-2-13 17:23:3 +0800
CreateTime: 2018-2-13 17:23:3 +0800
categories: Win10 UWP
---

本文，如何使用smms图床上传图片，用到win10 uwp post文件，因为我是渣渣，如果本文有错的，请和我说，在本文评论，或发给我邮箱，请不要发不良言论

<!--more-->



<div id="toc"></div>

找到一个很好的图床，sm.ms

可以简单使用post上传文件，我就做了一个工具，可以把图片上传，使用只需要

```csharp
            //传入文件
            smms.Model.Imageshack imageshack = new Imageshack()
            {
                File=File,
            };
            //上传完成事件，其中str为sm.ms返回，一般为json
            //Reminder是例子，可以根据具体修改，注意要同步CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync
            imageshack.OnUploadedEventHandler += (sender, str) => Reminder = str.Replace("\\/","/");
            //上传
            imageshack.UpLoad();
```

我将会把我做的发现的和大家说

## 进行HttpClient post参数错误

从“Windows.Web.Http.HttpStringContent”转换为“System.Net.Http.HttpContent”

原因

用了`System.Net.Http.HttpClient`其实HttpStringContent是可以在错误看到，不是System.Net.[Http](Http )

方法

使用

```csharp
           Windows.Web.[Http.HttpClient](Http.HttpClient ) web[HttpClient=](HttpClient= )
                new Windows.Web.[Http.HttpClient();](Http.HttpClient(); )

           Windows.Web.[Http.HttpStringContent](Http.HttpStringContent ) [httpString=](httpString= )
                new HttpStringContent("http://blog.csdn.net/lindexi_gd");
            await web[HttpClient.PostAsync(new](HttpClient.PostAsync(new ) Uri(url), [httpString);](httpString); )
```


## win10 uwp post 上传文件

我们可以使用HttpMultipartFormDataContent上传
其中我们需要从文件转流，打开StorageFile，把它转换[HttpStreamContent](HttpStreamContent )

            var fileContent = new HttpStreamContent(await File.OpenAsync(FileAccessMode.Read));

然后我们要fileContent.Headers.Add("Content-Type", "application/octet-stream");

我们可以把httpMultipartFormDataContent加上fileContent，看到sm.ms

|参数名称|类型|是否必须|描述|
|--|--|--|--|
|smfile|File|是|表单名称。上传图片用到|
|ssl	|Bool|	否|	是否使用 https 输出，默认关闭|
|format	|String|	否|	输出的格式。可选值有 json、xml。默认为 json|
|domain|	Int|	否|	图片域名。可选|

我们就修改`Add(IHttpContent content, System.String name, System.String fileName);` name "smfile"

    httpMultipartFormDataContent.Add(fileContent, "smfile", File.Name);

使用`await webHttpClient.PostAsync(new Uri(url), httpMultipartFormDataContent);`

因为需要拿到上传图片

```csharp
var str = await web[HttpClient.PostAsync(new](HttpClient.PostAsync(new ) Uri(url), [httpMultipartFormDataContent);](httpMultipartFormDataContent); )
            ResponseString = str.Content.ToString();
            OnUploadedEventHandler?.Invoke(this,ResponseString);
```

## 所有代码

[https://github.com/lindexi/Imageshack/tree/master/smms](https://github.com/lindexi/Imageshack/tree/master/smms )





