* Doctype作用？标准模式与兼容模式各有什么区别?

  （1）<!DOCTYPE>声明位于位于HTML文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。

  （2）标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。

* HTML5 为什么只需要写`<!DOCTYPE HTML>`？
	
	HTML5不基于SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为(让浏览器按照它们应该的方式来运行)
	而HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型

* HTML空元素

	从前我只知道HTML元素分为块元素和行内元素，还有一种分发是空元素，空元素就是没有内容的元素，必须br，hr，img，input，link，meta
	未来这些元素都要使用/，这是标准写法 `<br />`

* HTML5的离线缓存

	在用户没有与因特网连接的时候，可以正常访问站点或应用，在用户与因特网连接时候，更新用户机器上的缓存文件。
	原理是基于一个新建的.appcache文件的缓存机制(不是缓存技术)
    使用方法是在html标签中添加后缀为manifest的缓存设置文件，一般manifest的名字和对应的html相同即可
    
    ```
	<html lang="en" manifest="index.manifest"></html>
	CACHE MANIFEST   必要格式
	#v0.11                      注释
	CACHE:                     需要缓存的部分
	js/app.js
	css/style.css
	NETWORK:                跳过缓存的部分，每次都要向服务器重新获取
	resourse/logo.png
	FALLBACK:                 当缓存gg的时候，替代的页面
	//offline.html
    ```

	可以通过操作js手动更新缓存
	window.applicationCache.update();

* iframe有哪些缺点?

	1.iframe会阻塞主页面的onload事件

	2.搜索引擎的检索程序无法解读这种页面，不利于SEO(search engine optimistion)搜索引擎优化

	3.iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载
	使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript动态给iframe添加src属性值，这样可以绕开上面两个问题

* 自然样式标签和语义样式标签

	Physical Style Elements -- 自然样式标签
	b, i, u, s, pre
	Semantic Style Elements -- 语义样式标签
	strong, em, ins, del, code
	应该准确适用语义样式标签，但不能滥用，如果不能确定时首选使用自然样式标签

* 一次完整的HTTP请求从客户端到服务器端所经过的各个环节
	
	首先，我们要知道HTTP是一个应用层的超文本传输协议，其通信往下走还是TCP那一套，因此什么三次握手啊，通信时候保持TCP连接啊都是这个环节中的部分
	1. 输入域名，浏览器会新开启一个线程去访问DNS服务器，获取与域名相匹配的IP地址
	2. TCP的三次握手，建立TCP链接，形成一个session会话
	3. 浏览器发送request包，服务器接收后对其进行解析，如果请求资源中包含动态语言的内容，服务器会调用动态语言的解释引擎对其进行解析
	4. 请求按router被转发给一个定义好的handle处理，可能包含字符串操作
	5. ssr
	6. 返回response请求
	7. 浏览器会渲染response中的body，接下来就是DOM树的建立，同步解析css，最后就是render页面

* input属性disabled和readonly的区别
	
	readonly只针对input(text/password)和textarea有效，而disabled对于所有的表单元素都有效，但是表单元素在使用disabled后，当我们将表单以post或get方法提交之后，这个元素的值不会被传递出去，而readonly会将该值传递出去，因为当设置为disabled时候，该元素无法获得焦点，而设置为readonly的时候，使能够获得焦点的，因此会被当作form中的一项提交

* 点击图片的某些位置，是可以跳转到你对应的网站的，需要使用到map和area
    
    ```html
    <img src="./avatar.jpg" usemap="#map">
    <map name="map">
    	<area shape="rect" coords="0, 0, 10, 10" href="https://www.baidu.com" target="_blank">
    </map>
    ```

* html在title中显示小图标的方法

	`<link rel="shortcut icon" href="./webp.ico">`

* 在HTML页面内兼容移动端的操作

    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-saclable=no">

	如果不在HTML页面中加入这句话，css中@media无法根据device-width做兼容，js代码中获取的页面宽度一直为900



* 知道浏览器的requestAnimationFrame这个API么？
	
	requestAnimationFrame在React和Angular中都有使用	

	可能有很多大兄弟觉得transform, keyframe或者transition很好用，当然我也是这么觉得的，然后有一天我看到一个面试题目，大概是往一个ul里面插入30000个li，我想到了documentFragment，用的是setTimeout，然后看了解答发现用的是polyfill，我用的是requestAnimationFrame的备胎！！！

	使用setTimeout的问题是，画面的更新频率要达到每秒60次才能让肉眼看到流畅的动画效果，因为很多浏览器的频率为60HZ，这也是为啥我的setTimeout时间间隔经常使用的是1000 / 60的原因

	下面给出一段由requestAnimationFrame实现进度条由0-100的代码
	
    	var ndProgress = document.getElementById('js-test');
    	var proNum = 0;
    
    	function step () {
    		if (proNum < 100) {
    			proNum += 1;
    			ndProgress.style.width = proNum + '%';
    			ndProgress.innerHTML = proNum + '%';
    			window.requestAnimationFrame(step);
    		}
    	}
    	
    	requestAnimationFrame(step);

	requestAnimationFrame简介：http://www.cnblogs.com/Wayou/p/requestAnimationFrame.html

	requestAnimationFrame polyfill：https://gist.github.com/paulirish/1579671

	让我知道这个api的面试题目：https://zhuanlan.zhihu.com/p/26420034

* requestAnimationFrame浏览器做了什么优化

    [浅析requestAnimationFrame](http://taobaofed.org/blog/2017/03/02/thinking-in-request-animation-frame/)    
    
    requestAnimationFrame 的优点

    * requestAnimationFrame 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率
    * 在隐藏或不可见的元素中，requestAnimationFrame 将不会进行重绘或回流，这当然就意味着更少的 CPU、GPU 和内存使用量
    * requestAnimationFrame 是由浏览器专门为动画提供的 API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了 CPU 开销    

    简单来说，requestAnimationFrame 的原理如下所示

    * 注册回调函数
    * 浏览器更新时触发animate
    * animate会触发所有注册过的callback

    这里的工作机制可以理解为所有权的转移，把触发帧更新的时间所有权交给浏览器内核，与浏览器的更新保持同步。这样做既可以避免浏览器更新与动画帧更新的不同步，又可以给予浏览器足够大的优化空间
    
* 各大浏览器的内核

	1. IE一直使用的是trident
	2. safari, chrome使用的是webkit
	3. opera原来使用的是presto（速度超过了当年的webkit），然后13年后使用blink(webkit的分支)
	4. Firefox使用的是gecko内核

* 在Chrome控制台用dataset设置data-\*属性（需要使用驼峰式命名方法，浏览器会自动将其转为data-\*形式）

    	$0.dataset.aBC = 1;
    	<div class="" id="lga" data-a-b-c="1">...</div>
    	$0.dataset.aBC;  // 1
    	$0.getAttribute('data-a-b-c'); // 1 

	css中也可以通过自定义data-*属性选中Element
	[data-a-b-c="1"] {
		color: red;	
	}

* DOMContentLoaded

	当初始HTML文档被完全加载解析时，DOMContentLoaded事件被触发，而无需等待样式表、图像和子框架完成加载。另一个不同的事件load应该仅用于检测一个完全加载的页面。在使用DOMContentLoader更加合适的情况下使用load是个非常流行的错误，所以要谨慎

* 在本地当前目录启动静态文件服务器

	可以用于安装apk的文件，当然正道是通过adb shell安装，不过这样暴力省事啊，python -m SimpleHTTPServer 8080 或者全局安装anywhere这个朴灵大大写的库

* target和currentTarget的区别

	target是事件的真实发生节点，currentTarget是绑定事件处理程序的节点

* 内容安全策略(CSP)
	
	预防XSS的一个重要的方法就是配置白名单，CSP就是一种快速的配置白名单的方法

	`meta(http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline' 'unsafe-eval' www.google-analytics.com")`
	
	上面是一个例子，pug模版写法，self代表本域名的script，unsafe-inline代表内联的script，unsafe-eval用于执行eval(webpack打包出来很多eval。。.) 最后GA的域名是允许使用GA统计

* a标签将href替换为data-href，在谷歌搜索页面中能够跳转，在百度则不能，应该是谷歌自动错误修正

* 使用meta标签来调节浏览器的渲染方式，告诉浏览器用哪种内核渲染，360双核浏览器就是在ie和chrome之间来回切换，现在使用meta标签来强制使用最新的内核渲染页面

    `<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">`

* 看vue的源码发现parse模版的时候解析了两种注释节点，记录一下
    
    * \<!-- comment -->
    * \<![ comment ]>