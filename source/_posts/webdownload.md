---
title: "前端下载资源方法"  
date: 2019-03-02
tags: 
	- h5
---

### &nbsp;
日常场景中，在下载资源时，常用的方法是，将静态资源上传至服务器，之后通过a标签的href，获取资源的路径开始下载，这其中免不了需要后端的一番配置，但是如果需要下载的只是一个几行文字的txt文件，亦或是一张图片，这样就显得有些繁琐。
<!-- more -->
```
<a href='https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png' download="google_logo.png">下载</a>
```
在浏览器中，这段代码表示，此为可下载链接，只需点击就能下载链接地址的图片。
>#### [download(MDN)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a):
>此属性指示浏览器下载URL而不是导航到URL，因此将提示用户将其保存为本地文件。
>如果属性有一个值，它将在保存提示中用作预先填写的文件名 (用户仍然可以根据需要更改文件名)。对允许的值没有限制，但是/和\被转换为下划线。大多数文件系统限制文件名中的一些标点符号，浏览器会相应地调整建议的名称。

如果仅仅是向浏览器表明链接地址为可下载，`download`属性看起来可有可无，但是如果有方法让a标签不是指向链接地址，而是直接等于需要下载的数据，使标签自身就是需要下载的文件，那么我们甚至可以不需要知道文件所在的地址（甚至文件本身并不存在），就可以完成下载。

[__`Data URLs`__](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/data_URIs)或许可以将这个想法转化为现实，**Data URLs**，即前缀为 `data:` 协议的的URL，其允许内容创建者向文档中嵌入小文件。

这意味着，我们可以将文件数据通过url的形式来将其表现。
##### 首先看一下语法：
Data URLs 由四个部分组成：前缀(data:)、指示数据类型的MIME类型、如果非文本则为可选的base64标记、数据本身：
>data:[<mediatype>]\[;base64\],<data>  

简单的来说：一个内容为“Hello,  World”的txt文件可以表示为：
​    `data:,Hello%2C%20World`

 >试着将其作为a标签中的链接地址：
 > `<a href='data:,Hello%2C%20World' download="hello_world.txt">下载</a>` 

点击a标签下载文件`hello_world.txt`后，将其打开，可以看到文件内容的确是我们想要的**Hello,  World**
>另外通过js的全局函数`encodeURIComponent`，同样可以方便地将文本编码成data url数据

至此，我们已经可以成功的将文本作为数据保存在a标签中，并且可以方便地点击下载文本文件。

---
图片文件的下载，与文本下载类似，只要想办法将图片转化为data url形式，之后作为a标签href的值即可，这里简单说一下思路：
>	1. 通过canvas的drawImage方法，将图片输出到canvas中。
>	2. 使用canvas中的toDataURL，通过参数指定需要输出为dataurl的范围，即可将所选范围中的图片数据输出为dataurl。
>	3. 将输出的数据传入a标签中。

---

