---
title: "Vue制作豆瓣电影（调用api测试）"  
comments: true  
tags: 
	- vue
	- api
---

由于豆瓣api跨域问题，因此不能直接通过ajax请求访问，我们通过vue-cli提供给我们的代理（proxy）进行配置即可，打开config/index.js，配置代理proxyTable属性如下：
```
//在proxyTable这个属性中，配置target属性为我们要代理的目标地址。
proxyTable: {
    '/api': {
      target: 'http://api.douban.com/v2',
      changeOrigin: true,
      pathRewrite: {
        '^/api': ''
      }
    }
  }
```
“豆瓣API是有请求次数限制的”，这会引发图片在加载的时候出现403问题，视图表现为“图片加载不出来”，控制台表现为报错403。

其实是豆瓣限制了图片的加载，我自己用了一个办法把图片缓存下来： 
只要在请求到的图片链接前面加上‘https://images.weserv.nl/?url=’即可（注：这是一个专门缓存图片的网址），可能会有点慢。

```
getImages( _url ){
      if( _url !== undefined ){
        let _u = _url.substring( 7 );
        return 'https://images.weserv.nl/?url=' + _u;
      }
    }
```