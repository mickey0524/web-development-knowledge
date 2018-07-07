* post请求的四种提交方式

1. form-data

	就是http请求中的multipart/form-data,它会将表单的数据处理为一条消息，以标签为单元，用分隔符分开。既可以上传键值对，也可以上传文件。当上传的字段是文件时，会有Content-Type来说明文件类型；content-disposition，用来说明字段的一些信息；
	由于有boundary隔离，所以multipart/form-data既可以上传文件，也可以上传键值对，它采用了键值对的方式，所以可以上传多个文件。
	
2. x-www-form-urlencoded

	 就是application/x-www-from-urlencoded,会将表单内的数据转换为键值对，比如,name=Java&age = 23
	 
3. raw

	可以上传任意格式的文本，可以上传text、json、xml、html等
	
4.  binary

	相当于Content-Type:application/octet-stream,从字面意思得知，只可以上传二进制数据，通常用来上传文件，由于没有键值，所以，一次只能上传一个文件
	
5. multipart/form-data与x-www-form-urlencoded区别
   
   multipart/form-data：既可以上传文件等二进制数据，也可以上传表单键值对，只是最后会转化为一条信息；
   x-www-form-urlencoded：只能上传键值对，并且键值对都是间隔分开的。

* XSS和CSRF简介

	XSS（Cross-site scripting）跨站脚本注入，不对服务器进行直接的攻击，转而攻击使用服务器的用户，最为常见的方法就是在用户的输入框内输入script表现包裹的javascript语句，下次其他用户访问网站的时候，就会被收到XSS攻击啦，比如在input输入框中输入如下语句
	
	```
	<script>
		while(true) {
			alert('233333');
		}
	</script>	
	```

	这样其他用户打开该网站的时候，就会一直有23333的弹窗。

	那么该如何预防XSS呢，如果我们不需要用户输入 HTML 而只想让他们输入纯文本，那么把所有用户输入进行 HTML 转义输出是个不错的做法。似乎很多 Web 开发框架、模版引擎的开发者也发现了这一点，Django 内置模版和 Jinja2 模版总是默认转义输出变量的。如果没有使用它们，我们自己也可以这么做。PHP 可以用 htmlspecialchars 函数，Python 可以导入 cgi 模块用其中的 cgi.escape 函数。如果使用了某款模版引擎，那么其必自带了方便快捷的转义方式
	
	那如果某些地方需要用户输入HTML呢，这个时候，就需要非常小心了，仅仅对script标签做过滤是没有办法完全杜绝XSS的，随便在一个DOM节点上绑定一个onclick的事件处理函数或者使用display的iframe进行用户不知道的请求，都能实现XSS，有个比较方便的解决方法，增加白名单，我们不对用户的输入做直接存储，而是根据HTML的结构进行解析（HTML比XML的容错率高），然后将解析完毕的结果存入数据库，读取拼接数据的时候，将节点name和白名单中进行比较，大大降低XSS的可能性
	
	CSRF（Cross-site request forgery）跨站请求伪造
	
	XSS是CSRF的一种，把通过XSS实现的CSRF称为XSRF，CSRF和XSS的攻击方式有一些相似，都不是直接对服务器进行攻击，而是攻击使用服务器的用户，预防CSRF的基本方法是使用POST请求代替GET请求进行写入的操作，然而黑客还是可以通过不可见的表单进行攻击，这时候就需要行之有效的请求令牌来预防CSRF了，举个栗子，django中的{% csrf_token %}，服务器会在session中写入csrftoken，请求的时候，需要在请求头部写入X-CSRFTOKEN，值为cookie中的csrftoken, 也可以使用加密的cookie，但是如果网站没有预防XSS的话，执行了外站的脚本，就可能造成cookie泄露，还是不如token
	
	[XSS,CSRF参考](http://www.cnblogs.com/lins05/archive/2012/12/02/2797996.html)

* HTTP缓存机制

    [HTTP 缓存 | Web | Google Developers](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn)
    
    [彻底弄懂 HTTP 缓存机制 —— 基于缓存策略三要素分解法](http://geek.csdn.net/news/detail/131318)

* TCP的三次握手和四次挥手
    
    * 三次握手

        * 客户端发送带有SYN标志的数据包
        * 服务端收到了数据包之后，发送带有SYN/ACK的数据包
        * 客户端收到ACK之后，向服务端发送ACK，开始通信

    * 四次挥手

        * 客户端发送一个FINISH给服务端
        * 服务端收到之后，返回一个ACK给客户端
        * 服务端发送一个FINISH给客户端，服务端断开连接
        * 客户端收到之后，发送ACK给服务端，客户端断开连接