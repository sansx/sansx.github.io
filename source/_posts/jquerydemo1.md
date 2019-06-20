------
title: JQuery实例--1（在线聊天）
date: 2018-02-14
------


### 实例展示
**在线聊天室**
![](https://cl.ly/361J3N3W0S43/2.gif)

>《锋利的jquery》主题内容之后，附带了一个在线聊天室的实例代码参考，在理解了大致思路之后，我按照自己的想法制作了一个聊天室，后续可能会有扩展（待定）
<h3>1.实现思路</h3>
* 简单的来说就是将本地信息的上传到网络，之后同步信息到另外一个页面；
* 因为js需要借助后端来交互数据，所以这次后端选用php;
* 大致流程：
   <!-- more -->
   1. 打开网页输入名称（**不能重复**），用于在数据库检测信息来源
   2. 进入聊天页面，前端检测是否有新的信息输入   
>有两种情况：1.本页面输入；2.对方页面输入
   3. 如果有，数据将通过后台传入数据库，并返回数据库中信息的数量，
通过比较前端页面中显示的信息数量，来判断是否要更新页面
   4. 当页面没有新的数据XX秒之后，关闭页面

<h3>2.代码</h3>
1. 数据库
   * 这次数据库只是作为信息传递的跳板，不需要很复杂，`user`用户名作为代号显示信息来源，`info`暂时储存传递的信息，默认`id`主键自增,设置编码为utf-8;

		CREATE TABLE IF NOT EXISTS `chats`(  
	`id` int(6) UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,  
	`info` mediumtext NOT NULL,  
	`user` VARCHAR(100) NOT NULL  
    )CHARSET=UTF8;

2. 后台
   + 简单的数据读写，根据前端传入**关键词**选择操作
   
3. 前端
   + js库为  jquery*
   + HTML结构：  

	&lt;div id=&quot;box&quot;&gt;  
	　　&lt;div id=&quot;outb&quot;&gt;  
		　　　　&lt;div id=&quot;chatinfo&quot;&gt; &lt;/div&gt;  
	　　&lt;/div&gt;  
	　　&lt;div id=&#x27;chatipt&#x27;&gt;  
	　　　　&lt;input type=&quot;text&quot;   name=&quot;&quot; placeholder=&quot;点击输入&quot; /&gt;
	　　　　　&lt;button type=&quot;button&quot;&gt;发送&lt;/button&gt;  
	　　&lt;/div&gt;  
	&lt;/div&gt;

	+ 截取一段js代码  
``` bash
			function getinfo(){ //获取信息
				if (state==1) { //检测状态state,如果等于1表示之前的ajax还未执行完毕，则跳过本次
					return;
				}
				state=1;
				$.ajax({
					url:'php/chatdata.php',
					type:'post',
					data:{
						name:name,
						info:'',
						num:$('#chatinfo div').length//获取当前页面信息数量，传递后台
					},success:function(a){
						if (a=='') {if (timer==''){timer=setTimeout(candel,'300000');}return;};//重置计时器
						for(var i in JSON.parse(a)){//将后台数据输出到前端页面
							$('#chatinfo').append('<div><p class=yinfo>'+JSON.parse(a)[i].info+'</p></div>');
							var bb = $('#chatinfo')[0].offsetHeight-$('#chatinfo').parent().height();
							aa = -bb;
							$('#chatinfo').animate({'top':aa+'px'},200);
							clearTimeout(timer);
							timer='';
						};
						echo(JSON.parse(a),state);//封装的控制台输出函数，相当于console.log()
					},complete:function(){
						state=0;//成功获取后台信息，将state设置为0，表示可以继续获取数据
					}
				})
			}
```
> 最主要的是前端信息的检测与处理  

**[查看聊天室](http://xthtx.site/chatonline/chathome.html)**  

**[查看源码->github](https://github.com/sansx/web/tree/master/chatonline)**  



----
![](https://d26dzxoao6i3hh.cloudfront.net/items/3u1r282K3s2U1k2S3f2Z/j2.gif)