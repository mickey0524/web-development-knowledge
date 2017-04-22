## 前言

本文由我收集整理了一些前端知识点，参考自各前端大大的博客，github以及各社区的热点题目，前端的知识点多而繁杂，将知识点集中起来温故而知新是极好的。

## 目录

* [HTML](#HTML) 
* [CSS](#CSS) 
* [JavaScript](#JavaScript)
* [jQuery](#jQuery)

<h2 id="HTML">HTML</h2> 

* Doctype作用？标准模式与兼容模式各有什么区别?

  （1）、<!DOCTYPE>声明位于位于HTML文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。

  （2）、标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。

* HTML5 为什么只需要写 <!DOCTYPE HTML>？
	
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
	  	/ /offline.html
```

	可以通过操作js手动更新缓存
	window.applicationCache.update();

* iframe有哪些缺点?

	1.iframe会阻塞主页面的Onload事件

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

<h2 id="CSS">CSS</h2>

* BFC(Box Formatting Context) 块级格式化上下文

	尝试了一下觉得很好玩

	http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html

	上面这个url中很详细的讲解了有关BFC的栗子和原理

	BFC布局的规则:
	
	  1. 内部的Box会在垂直方向，一个接一个地放置。
	  2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
	  3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
	  4. BFC的区域不会与float box重叠。
	  5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
	  6. 计算BFC的高度时，浮动元素也参与计算

	哪些元素会生成BFC？
	
	  1. 根元素
	  2. float属性不为none
	  3. position为absolute或fixed
	  4. display为inline-block, table-cell, table-caption, flex, inline-flex
	  5. overflow不为visible     这一条用的最多，设置overflow为hidden就能触发该block成为BFC

	BFC的用处
	上面url中写的非常好
	
	1. 可以用于自适应两栏布局
	2. 可以消除一个BFC中margin重合的问题
	3. 可以清楚内部浮动（因为计算BFC高度的时候，浮动元素也被计算在内）

* 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的?

	盒模型：内容(content) 填充(padding) 边界(margin) 边框(border)
	
	低版本的IE的content部分将border和padding计算了进去

* 经常遇到的浏览器的兼容性有哪些？解决方法是什么，常用的hack技巧?

	1. png24位的图片在ie6浏览器上出现背景，解决方案是做成png8
	2. 浏览器默认的margin和padding不同。解决方案是加一个全局的* { margin: 0; padding: 0 }来统一
	3. ie下，可以使用获取常规属性的方法来获取自定义属性，也可以使用getAttribute()获取自定义属性
	Firefox下，只能使用getAttribute()来获取自定义属性，因此最好使用getAttribute获取自定义属性
	
	4. IE下，event对象有x，y属性，但是没有pageX，pageY属性；Firefox下，event对象有pageX，pageY属性，但是没有x，y属性，很伤
	
	5. Chrome中文界面下默认会将小于12px的文本强制按照12px显示，可通过加入css属性 -webkit-text-size-adjust: none;解决
	
	6. IE兼容hack技术（渐进识别的方式，从总体中逐渐排除局部）
```
	.bb {
	background-color: red; /*所有识别*/
	background-color: #00deff\9; /*IE6，7，8识别*/
	+background-color: #a200ff; /*IE6，7识别*/
	_background-color: #1e0bd1; /*IE6识别*/
	}
```

* 八种创建等高布局的方法
	http://www.w3cplus.com/css/creaet-equal-height-columns

* 为什么要初始化CSS样式

	因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面之间的差异。当然，初始化样式会对SEO有一定的影响，但鱼与熊掌不可兼得，但力求影响最小的情况下初始化
	最简单的初始化方法： * { padding: 0; margin: 0 } (强烈不建议)
	淘宝的样式初始化方法：
```
  body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
  body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
  h1, h2, h3, h4, h5, h6{ font-size:100%; }
  address, cite, dfn, em, var { font-style:normal; }
  code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
  small{ font-size:12px; }
  ul, ol { list-style:none; }
  a { text-decoration:none; }
  a:hover { text-decoration:underline; }
  sup { vertical-align:text-top; }
  sub{ vertical-align:text-bottom; }
  legend { color:#000; }
  fieldset, img { border:0; }
  button, input, select, textarea { font-size:100%; }
  table { border-collapse:collapse; border-spacing:0; }
```

* CSS里的visibility属性有个collapse属性值是干嘛用的

    对于普通元素visibility:collapse;会将元素完全隐藏,不占据页面布局空间,与display:none;表现相同. 如果目标元素为table,visibility:collapse;将table隐藏,但是会占据页面布局空间. 仅在Firefox下起作用,IE会显示元素,Chrome会将元素隐藏,但是占据空间

* 什么是外边距合并

	外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距，合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者，下面url中详细介绍了外边距合并
	http://www.w3school.com.cn/css/css_margin_collapsing.asp

* 请解释一下为什么需要清除浮动？清除浮动的方式

	清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷是我们页面后面的布局不能正常显示。
	
	1. 父级div定义height
	2. 父级div也一起浮动
	3. 父级div变为BFC，这样计算高度的时候会把浮动元素算进去
	4. 常规的使用一个class
	```
		.clearfix: before, .clearfix: after {
			content: '';
			display: table;
		}
		.clearfix: after {
			clear: both;
		}	
		.clearfix {
			*zoom: 1;	为了兼容IE出发haslayout
		}

* zoom: 1的清楚浮动的原理

	清楚浮动，出发hasLayout

	Zoom属性是IE浏览器的专有属性，它可以设置或检索对象的缩放比例。解决IE下比较奇葩的问题

	当设置了zoom的值后，所设置的元素就会扩大或者缩小，高度宽度就会重新计算了，这里一旦改变zoom值时其实也会发生重新渲染，运用这个原理，也就解决了ie下子元素浮动时候父元素不随着自动扩大的问题。

* 浏览器是怎样解析CSS选择器的
	
	样式系统从关键选择器开始匹配，何为关键选择器，就是选择器的最后面的部分，如果规则拥有ID选择器作为其关键选择器，则不要为规则添加标签，过滤掉无关的规则，这样速度会变快，话说回来，样式系统从关键选择器开始匹配，然后左移查找规则选择器的祖先元素，只要选择器的子树一直在工作，样式系统就会持续左移，直到和规则匹配，或者是因为不匹配而放弃该规则	

* 元素竖向的百分比设定是相对于容器的高度吗？

	这个知识点很有意思，之前我觉得肯定是相对于容器的高度，后来才知道是要分height和padding-top，margin-top，padding-bottom，margin-bottom这些属性设置百分比的话参照的是容器的宽度而不是高度，很神奇，height属性还是和常理一样参照的是容器的高度。

* 响应式设计的基本原理是什么？如何兼容低版本的IE？

	基本原理是媒体查询 @media

	低版本的IE之所以不支持响应式是因为其不能识别选择器，可以依靠外部js来实现兼容，例如respond.js

* 如何修改chrome记住密码后自动填充表单的黄色背景？

	黄色背景是因为chrome会默认给自动填充的input表单加上input:-webkit-autofill属性

```
	input:-webkit-autofill textarea:-webkit-autofill, select:-webkit-autofill {
		background-color: #FAFFBD;
		background-image: none;
		color: white;	
	}
```

	可以使用足够大的纯色内阴影来覆盖input输入框的黄色背景

```
	input:-webkit-autofill {
		border-shadow: 0 0 0px 1000px white inset;
	}
```

* line-height的一些理解

	如果子元素自己有line-height, 则肯定是和子元素自己的font-size去做相乘得到子元素的line-height

	如果子元素自己没有line-height, 也没有font-size，则无论哪种写法，都是用父亲元素的font-size和line-height去相乘

	如果子元素有font-size没有line-height，则父元素line-height为px直接继承，父元素为%和em，用父元素的font-size和line-height相乘，父元素为数字, 用父元素line-height和子元素的font-size相乘得到子元素的line-height

* 如果要手写动画，你认为最小时间间隔是多久，为什么？

	多数显示器默认频率为60Hz，即1s刷新60次，所以理论上最小间隔为1/60 * 1000ms = 16.7ms

* 什么是CSS预处理器/后处理器

	1. 预处理器例如：less，sass，stylus，用来预编译sass或less，增强了css代码的复用性，还有层级，mixin，变量，循环，函数等，具有很方便的UI组件模块化开发能力，极大的提高了工作效率
	2. 后处理器例如：PostCSS，通常被视为在完成的样式表中根据CSS规范处理CSS，让其更有效；目前最常用的是给CSS属性添加浏览器私有前缀，实现跨浏览器的兼容性问题

<h2 id="JavaScript">JavaScript</h2>

* 如何区分{} 和 [] 

	使用typeof得到的都是object

	最好的方法是使用object原型的toString方法

```
	function isArray(o) {
	    return Object.prototype.toString.call(o) === ‘[object Array]‘;
	}
```

* 使用jQuery，找到id位selector的select标签中有用data-target属性为isme的option的值

    `$('#selector option[data-target=isme]')`

* JavaScript有几种数据类型？

	栈：原始数据类型（Undefined, Null, Boolean, Number, String）

	堆：引用数据类型（对象，数组和函数）

	两种类型的区别是：存储位置不同
	
	原始数据类型直接存储在栈(Stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储

	引用数据类型存储在堆(heap)中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获取实体。

* 数组随机排序新姿势get
	
```
	var arr = [1, 2, 3, 4, 5, 6, 7, 8];
	arr.sort(function() {
		return Math.random() - 0.5;	
	});
```

* eval是做什么的？

	它的功能是把对应的字符串解析成JS代码并运行；
	应该避免使用eval，不安全，非常耗性能（2次，一次解析为JS代码，一次执行）

* ['1', '2', '3'].map(parseInt)答案是多少?

	很多人第一眼看到这个问题肯定回答是[1, 2, 3]，但是尝试一下，发现不是这样，是不是觉得很奇怪，就很气，下面我们来分析一下。

	map()函数的参数是一个函数，这个函数的参数为(item, index, array), 在这个等式中，parseInt为map的参数，那么map就会将上书三个参数传给parseInt

	下面我们来分析一下parseInt函数，这个函数可以将2-36进制的字符串转为10进制，它接受2个参数，parseInt(num, scale), 因此map函数传给其的三个参数取前两个参数
	
	1传递过来的参数是 (1,0) 0会被parseInt默认按10进制来计算，因此得到的结果是1
	
	2传递过来的参数是 (2,1) 由于parseInt接受的参数为2-36，因此无法解析，得到的结果是NaN

	3传递过来的参数是 (3,2) 有的小伙伴就会说啦，你看，现在是2-36范围内的值了，为啥还不对，23333，因为3要按2进制来解析，要怎么样解析嘛，因此得到的结果也是NaN

	因此['1', '2', '3'].map(parseInt)得到的答案就是[1, NaN, NaN]，是不是很神奇

* 事件是什么？(给出官方解释)IE和FireFox的事件机制有什么区别?如何阻止冒泡

	我们在网页中的某个操作(有的操作对应多个事件)。例如：当我们点击一个按钮就会产生一个事件，这是可以被Javascript所侦测到的行为。

	IE的事件事件处理只支持冒泡，Firefox的事件处理支持冒泡和捕获

	如何组织冒泡，也需要兼容性的考虑, event.stopPropagation()(旧IE方法 event.cancelBubble = true;)

* new操作符具体干了些什么？

	1. 创建了一个新对象，并且this变量引用该对象，同时还继承了该函数的原型
	2. 属性和方法被加入到this引用的对象中
	3. 新创建的对象由this所引用，并且最后隐式的返回this对象

* Javascript中，有一个函数，执行对象查找时，永远不会去查找原型，这个函数是

	hasOwnProperty

	javascript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有制定名称的属性，该方法不会去对象的原型链中查找
	
	object.hasOwnProperty(attribute)

	我这有个方法可以判断属性是否为其原型链中的属性
```
	function isPropertyAttr(obj, proName) {
		return !obj.hasOwnProperty(proName) && proName in obj;
	}
```

* [].forEach.call($$("*"),function(a){a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)}) 能解释一下这段代码的意思吗？

	说实话，第一眼看到这么长的代码，肯定是蒙蔽的，所以只能按部就班一步一步分析啦

	$$('*')这是chrome的api，用于获取页面上所有元素，$$('*')相当于document.querySelectorAll('*')，得到一个NodeLists，这不是真正的数组，因此需要采用[].forEach.call来改变this，也可以使用Array.property.forEach.call，前者少些一些代码

	后面代码可以很清晰的看到，是改动css的outline获得彩色轮廓，outline游离于盒子模型之外，因此不会影响元素之前的布局

	1<<24，css中，颜色值为6位16进制值，因此使用Math.random() * (1 << 24)可以得到范围内的一个小数，再采用~~两次取反得到整数部分，最后将10进制的数转为16进制，即大功告成啦2333

* 模块化开发怎么做？

	我一看到这个问题，想到的就是Nodejs的commonjs模块化，模块化将很多地方可能用到的代码块封装起来，隐藏具体的变量和实现方法，暴露出接口。

	然后联想到立即执行函数，reture一个对象变量，暴露出接口
```
	var module = (function() {
		var _count = 0;
		var m1 = function() {
		}
		var m2 = function() {
		}
		return {
			m1 : m1,
			m2 : m2
		}
	})();
```
	这样就暴露除了m1, m2两个接口，隐藏了模块的具体实现

* document.write 和 innerHTML的区别
	
	document.write能够重绘整个页面

    innerHTML能够重构部分页面

* 如何判断当前脚本运行在浏览器还是node环境中?

	`this === window` ? 'brower' : 'node'`
	node中this应该等于global

* 实现一个页面操作不会整页刷新的网站，并且能在浏览器前进、后退时正确响应。给出你的技术实现方案？

	我觉得这道题考的就是ajax干掉浏览器前进后退键的知识点，因为看到题目，首先想到的肯定是ajax优秀的部分刷新机制，然后再往后面看就会想到上面的问题，那么如何解决呢

	1. 加入页面操作改变了URL，可以在每个url后面加上hash，然后监听onHashChange，监听到变化后，重新执行数据拉取
	2. 使用Html5的Hisroty API 具体实现是使用pushState popState window.location.href这几个api
	3. 使用history.js

* 移动端最小触控的区域是多大?

	这个我也不知道，去百度了一下，好像ios是44pt * 44pt，android是48dp * 48dp，下次碰到了有机会再补充

* 把Script标签放在页面的最底部的body封闭之前和封闭之后有什么区别？浏览器会如何解析它们？

	==我以前接触的问题都是把script放在head中还是放在body标签结束之前(阻塞加载的问题)，从结果来看，放在body标签之内和body标签之外并没有什么区别，但是貌似有规则规定过html标签下只能有head和body，其中的script也会被错误修复机制放置到body内部去，有哪位老哥确切知道这个问题，可以告诉小弟一波~

* 移动端的点击事件有延迟，时间是多久，为什么会有？

	click有300ms延迟，为了实现safari的双击事件的设计，浏览器要知道你是不是要双击操作

	我想到最粗暴的方法就是user-scalable=no，禁用缩放美滋滋

* 用js实现千位分隔符？

	这个题目有很多种解决的方法，可以使用三位循环，字符串数组分隔，也可以使用正则，这里我给出正则的实现方式，其他两种较为简单就不给出实现方法了

```
	function changeFormat(num) {
		if (!num) {
			return ;
		}
		num = num.toString();
		return (num.indexOf('.') == -1) ?
			num.replace(/(\d)(?=(\d{3})+$)/g, function($0) {
				return $0 + ',';
			}) :
			num.replace(/(\d)(?=(\d{3})+\.)/g, function($0) {
				return $0 + ',';
			});
	}
	console.log(changeFormat(1234567.90));
	console.log(changeFormat(123123123));
```
	可能有同学会问？=是什么，？=表示正向引用，可以作为匹配的条件，但是匹配的内容不获取，并且作为下一次循环的开始，下面以123123123来分析一下
	第一次(\d)匹配到了3, 因为后面要满足(\d{3})+，后面模式匹配到的是456789，依此类推，匹配到的就是3和6，得到的结果就是123,456,789，需要注意的就是整数匹配时候，由于不像小数一样最后总有.限制条件，需要使用$关闭懒惰匹配，不然得到的就是1,2,3,4,5,6,789

* 我们给一个DOM元素同时绑定两个点击事件，一个用捕获，一个用冒泡。会执行几次事件，会先执行冒泡还是捕获？

	http://www.cnblogs.com/greatluoluo/p/5882508.html
	
	这里需要注意的是所有事件的顺序是：其他元素捕获阶段事件->本元素代码顺序事件->其他元素冒泡阶段事件

	下面来解释一下上面这句话的意思，如果点击的元素是本元素，那么执行的顺序就是捕获事件和冒泡事件定义的先后顺序，那个先定义，哪个就先执行，假如点击的元素是本元素的子元素，那么先执行捕获事件再执行冒泡事件，因为在W3C下，都是先从根元素执行捕获到目标元素，再从目标元素向上执行。

* 列举IE与其他浏览器之间不一样的特性？

	1. 其他浏览器触发事件的目标是event.target，而在IE中则是event.srcElement属性
	2. 获取字符代码，DOM需要使用charCode，而IE需要使用keyCode
	3. 阻止某个事件的默认行为，IE中将returnValue设为false，其他浏览器调用preventDefault()
	4. 停止事件冒泡，IE中设置cancelBubble为true，其他浏览器调用stopPropagation()

* 前端优化的大体思路？

	对普通的网站有一个统一的思路，就是尽量向前端优化、减少数据库操作、减少磁盘IO。
	
	1. 向前端优化指的是，在不影响功能和体验的情况下，能在浏览器执行的不要在服务端执行，能在缓存服务器上返回的不要到应用服务器，程序能直接取得的结果不要到外部取得，本机内能取得的数据不要到远程取，内存能取到的不要到磁盘去，缓存中有的不要去数据库查询。
	2. 减少数据库操作指的是，减少更新次数，缓存结果减少查询次数，将数据库执行的操作尽可能的让你的程序完成(例如join查询)。
	3. 减少磁盘IO指尽量不适用文件系统作为缓存、减少读写文件次数等。
	4. 程序优化永远要优化慢的部分，换语言是无法优化的。

<h2 id="jQuery">jQuery</h2>

* jQuery中jQuery和jQuery.fn的区别

	你可以把jQuery理解为jQuery的类，把jQuery.fn理解为jQuery的实例(那些通过jQuery选择器选择的元素都可以理解为jQuery的实例)

* jQuery.fn的init方法是否返回的this是指什么对象？为什么要返回this？

	当你使用jQuery去生成jQuery实例也就是$()的时候，你也就调用了jQuery.fn.init方法，其返回的this自然就是你找到的元素了，即jQuery.fn.init的实例，也就是一个类数组对象，至于为什么要返回this，不返回this，无法进行后续对jQuery元素的操作啊23333.

* jQuery中如何将数组转化为json字符串，然后再转化回来?

	我印象中好像是没有这个方法，下次发现了再更新啦

```
	$.fn.stringifyArray = function(array) {
		return JSON.stringify(array);	
	}

	$.fn.parseArray = function() {
		return JSON.parse(array);	
	}
```
	这样就给jQuery的实例添加了两个方法，`$("").stringifyArray(array)`这样使用就行

* 针对jQuery的优化方法？

	1. 基于class的选择器性能行对于ID选择器开销大得多，因为需要遍历所有DOM元素，因此，能用ID选择器的就不要用class选择器
	2. 频繁操作的DOM，先缓存起来再操作，用jQ的链式调用更好，反正我平时就特别喜欢链式调用233，我个人觉得链式调用更加清晰
	3. `for (var i = size, len = arr.length; i < length; i++)`for循环每一次都查找了arr数组的长度，如果在循环过程中长度不变的话，可以使用一个变量将其保存，这样循环可以跑的更快

* $.extend()和$.fn.extend()的区别?
	
	上面有个题目讲到$是jQuery类，$.fn是jQuery实例，然后我们又知道extend是扩展方法，那么自然而然就能理解$.extend()扩展的是jQuery类的方法，可以理解为jQuery类的静态方法，$.fn.extend()自然就是扩展jQuery实例的方法，下面举两个栗子来加强记忆

```
	$.extend({
		minValue: function(a, b) { return a > b ? b : a }
	})
	$.extend(1, 2); //1

	$.fn.extend({
		sayHello: function() {
			console.log('Hello');
		}	
	})
	$('#id').sayHello();  //$.fn是实例，那么该方法只能是实例来调用，$.sayHello会报错
```

* jQuery的属性拷贝(extend)的实现原理是什么，如何实现深拷贝?

	看到这里，可能会有疑问，上面一个问题明明讲了$.extend()就是为JQuery类扩展方法的，这里又说道属性拷贝，其实没有冲突，上文只是为了抓住重点进行解释。

	$.extend(boolean, target, object1, object2)

	第一个boolean值是指定是否采用深拷贝的标志，可以显示的指示为true，不能显示的指定为false，假如不用深拷贝的话，这个属性不赋值就行；

	target是进行属性拷贝的源对象，加入未定义，就默认为jQuery

	object1/object2即为一个一个的对象，后面对象中假如有和前面对象重复的定义，则后面的覆盖前面定义的属性

* jQuery的队列是如何实现的？队列可以用在哪些地方？

	说实话，我之前也没有用过jQuery的队列，正好借此机会，学习了一下关于队列的方法

	jQuery中，jQuery和jQuery.fn中都有queue相关的api，需要牢记的一点是队列中只能存函数

	首先我们看一下jQuery类中的队列

```
	function aaa() {
		alert(1);
	}

	function bbb() {
		alert(2);
	}

	$.queue(document, "q1", aaa);  //在document中创建一个队列q1，并往q1中添加aaa函数
	$.queue(document, "q1", bbb);  //在document下q1队列中添加bbb函数
	$.queue(document, "q1", [aaa, bbb]) //和上方代码效果一样，只是合起来写
	
	$.dequeue(document, "q1")  //取出document下q1队列中第一个函数并执行
``` 
	下面我们看一下jQuery实例中的队列

```	
	$(document).queue("q1", aaa); //入队
	$(document).dequeue("q1") //出队
	$(document).clearQueue("q1") //清空队列
```	
	至于队列有什么作用呢，大家可以想想，在接触jQuery动画的时候，是不是有动画依次执行的例子

```
	$(this).animate({ width: 300 }, 2000).animate({ left: 300 }, 2000);
```

* 谈一下jQuery中的bind(), live(), delegate(), on()的区别?

	bind()方法用来给当前已经存在的元素绑定事件处理函数(click, change等)，我个人喜欢用click()这样的写法

	live()，delegate()，on()我认为他们的作用是用来给当前还未被添加到DOM中的元素绑定事件处理函数，他们使用的都是事件委托原理，其中live()效率底下已经被废弃

	$('selector').live(type, callback) //live是使用document的事件委托，因此效率非常低
	
	$('parent').delegate(selector, type, callback) //delegate使用的是父亲元素的事件委托

	$('parent').on(type, selector, callback) 

* jQuery的slideUp动画，如果目标元素是被外部事件驱动，当鼠标快速地连续触发外部元素事件，动画会滞后的反复执行，该如何处理呢？

	利用stop()方法，如`$('#div').stop().animate({ width: 100 }, 100)`

	这里再回忆一下jQuery的stop()方法

	stop(): 停止当前执行的动画，如果动画存在队列的话，只能停止当前的，无法停止后续的动画队列

	stop(true)：停止当前执行的动画随后所有动画，且不会跳转到当前动画的终态

	stop(true, true)：停止当前执行的动画随后所有的动画，然后跳转到当前动画的终态，要记住，jQuery是无法跳转到动画队列最后一个动画的终态的，无论什么方法都不行

* jQuery一个对象可以同时绑定多个事件，这是如何实现的？

	1. 多个事件同一个函数：`$('div').bind('click change', function() {});`
	2. 多个事件不同函数：
	```
		$('div').bind({
			click: function() {},
			change: function() {}
		})
	```

