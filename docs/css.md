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
	2. 浏览器默认的margin和padding不同。解决方案是加一个全局的\* { margin: 0; padding: 0 }来统一
	3. ie下，可以使用获取常规属性的方法来获取自定义属性，也可以使用getAttribute()获取自定义属性
	Firefox下，只能使用getAttribute()来获取自定义属性，因此最好使用getAttribute获取自定义属性
	
	4. IE下，event对象有x，y属性，但是没有pageX，pageY属性；Firefox下，event对象有pageX，pageY属性，但是没有x，y属性，很伤
	
	5. Chrome中文界面下默认会将小于12px的文本强制按照12px显示，可通过加入css属性 -webkit-text-size-adjust: none;解决
	
	6. IE兼容hack技术（渐进识别的方式，从总体中逐渐排除局部）
	
		```css
		.bb {
		background-color: red; /*所有识别*/
		background-color: #00deff\9; /*IE6，7，8识别*/
		+background-color: #a200ff; /*IE6，7识别*/
		_background-color: #1e0bd1; /*IE6识别*/
		}
		```

* 八种创建等高布局的方法

	http://www.w3cplus.com/css/creaet-equal-height-columns

* css中的两栏布局和三栏布局

	http://blog.csdn.net/crystal6918/article/details/55224670

* 为什么要初始化CSS样式

	因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面之间的差异。当然，初始化样式会对SEO有一定的影响，但鱼与熊掌不可兼得，但力求影响最小的情况下初始化
	
	最简单的初始化方法： * { padding: 0; margin: 0 } (强烈不建议)
	
	淘宝的样式初始化方法：
	
	```css
  	body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, 	legend, button, input, textarea, th, td { margin:0; padding:0; }
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
	
	其实外边距合并就是因为前文讲过的BFC，因此消除外边距合并的办法就是让两个元素处于两个BFC就行了

* 请解释一下为什么需要清除浮动？清除浮动的方式

	清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使我们页面后面的布局不能正常显示。
	
	1. 父级div定义height
	2. 父级div也一起浮动
	3. 父级div变为BFC，这样计算高度的时候会把浮动元素算进去
	4. 常规的使用一个class

	
	```css
	.clearfix: before, .clearfix: after {
		content: '';
		display: table;
	}
	.clearfix: after {
		clear: both;
	}	
	.clearfix {
		*zoom: 1;	为了兼容IE触发haslayout
	}
	```

* zoom: 1的清楚浮动的原理

	清楚浮动，触发hasLayout

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

* 为什么阿里，腾讯等公司要把图片和css等资源放在单独的域名下？

	1. cookie free，读取这部分资源的时候，没有必要使用cookie，减少很多负载
	2. 负载均衡，便于管理
	3. 制作CDN，一劳永逸，不然图片这些挂在ip地址上的话，万一改了ip就麻烦了

* 回流，重绘以及回流重绘的优化

	http://www.cnblogs.com/nanshanlaoyao/p/5876018.html

* css中opacity和rgba的使用

	假如给父亲元素设置了opacity的话，所有的子元素都会受这个opacity的影响，如果要使得子元素不受父亲元素的限制，可以使用absolute绝对定位将原先的父子布局变为兄弟布局，这样父亲的opacity就不会影响孩子了

	rgba完美解决这个问题，只是ie不支持，就很气，rgba可以用于设置前景色，阴影色等各种颜色

* display为inline-block的元素之间会自动出现间距

	有很多种解决方法

	http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/

	我个人喜欢将父亲元素font-size设置为0，然后孩子元素重新设置font-size

* 前端什么时候用png，什么时候用jpg

	1. png支持透明，jpg不支持透明，需要使用透明的地方必须使用png
	2. png支持无损保存，jpg支持压缩不可逆保存，多次保存后质量会越来越差
	3. png比jpg大的多，能用jpg的地方尽量使用jpg

* 一个浮动元素如何与正常流中的内容发生重叠呢

	这需要用到margin的负值，例如，一个元素 float: left; margin-right: -10px;
	
	这里还有一个关于正常流元素内容显示的问题，当正常流和浮动元素重叠的时候，但是内容的显示就需要按行内元素和块元素区分记忆了

	假如正常流是行内元素，那么正常流的边框，背景和内容都将在浮动元素上面

	假如正常流是块状元素，那么正常流的内容会出现在浮动元素上面，边框和背景则在浮动元素之下

* 关于z-index的一些理解

	有关z-index 的一些理解（包括static / relative / absolute / fixed） 首先，static是默认状态，是不受top，left等和z-index的影响的，这种时候使用margin的负px移动div，会将下方的东西直接覆盖在当前div上 如果是使用relative等，则会先定位，然后再画图，就会使当前的div在下方div的上面

	父亲和孩子拥有相同z-index的时候，孩子会覆盖父亲

	兄弟拥有相同z-index的时候，后面的会覆盖前面的
	
* css属性clip裁剪矩形

	裁剪一个矩形，可以采用rect(top, right, bottom, left)
	
	top，right，bottom，left四个距离为四个方向距离左上角的距离，只能用auto或者长度，不能为百分数，right，bottom可以比父亲元素大，而top和left绝对不允许
	
	clip: rect(0px, 60px, 200px, 0px)

	需要记住的是，需要让这个属性生效的话，元素必须是absolute的

* css图片的预加载和懒加载
	
	http://blog.csdn.net/YiDaShi33/article/details/54316126

	css图片的预加载是在页面渲染之前，提前加载图片，当用户需要查看的时候可以直接从本地缓存中渲染，牺牲效率，提高用户体验

	css图片的懒加载(甚至是不加载)，指的是，将页面中的img标签的src属性设置为一个1px*1px的图片，而将图片真正的url放在标签的自定义属性中，类似data-origin，当图片真正被访问的时候，再将src设置为data-origin，极大的减轻了服务器的压力	

* css属性-伪类和伪元素的区别

	首先给出w3c对两者的定义:
	1. css伪类用于向某些选择器添加特殊的效果 :active :hover :focus等
	2. css伪元素用于将特殊的效果添加到某些选择器 ::before ::after等
	两者都与选择器有关，两者都是为了添加一些特殊的效果
	
* css限制多行，超出部分用省略号表示

	文本溢出我们经常用到的应该就是text-overflow:ellipsis了，相信大家也很熟悉
	
	```
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
	```
	但是这种方法并不能支持限制多行，WebKit内核的浏览器实现起来比较简单，可以通过添加一个-webkit-line-clamp的私有属性来实现

	```
	overflow : hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;
	```
	如果要做兼容的话，推荐使用clamp.min.js
	http://lomu.me/post/css-multiline-text-overflow

* animation动画

	animation默认是不保留状态的，如果需要的话，可以使用animation-fill-mode:(forwards/backwords/both);

* 用纯CSS实现以下效果，这逆向思维很赞

	![img](../images/1.gif)

	```js
	<!DOCTYPE html>
	<html>
	<head>
	  <meta charset="utf-8">
	  <title>JS Bin</title>
	</head>
	<body>
	  <ul>
	     <li></li>
	     <li></li>
	     <li></li>
	     <li></li>
	     <li></li>
	  </ul>
	</body>
	</html>
	
	*{
	  padding:0;
	  margin:0;
	}
	
	li{
	  list-style:none;
	  height:20px;
	  width:20px;
	  border-radius:50%;
	  margin:10px;
	  border:1px currentColor solid;
	}
	li:hover,li:hover~li{
	  background:gray;
	}
	ul{
	  display:flex;
	  direction:rtl;
	  width:fit-content;
	}
	```

* 用纯CSS实现下列效果

	![img](../images/2.gif)
	
	```js
	<!DOCTYPE html>
	<html>
	<head>
	  <meta charset="utf-8">
	  <title>JS Bin</title>
	</head>
	<body>
	  <div>
	    <label for="checkbox">
	        展示
	    </label>
	    <input id="checkbox" type="checkbox"></input>
	  <div class="modal">
	    <label class="close" for="checkbox">关闭</label>
	  </div>
	  </div>
	</body>
	</html>
	
	input[type="checkbox"]{
	  display:none;
	}
	label{
	  line-height:20px;
	  padding:2px 5px;
	  background:#aaa;
	  font-size:14px;
	  color:white;
	  box-shadow: 0 0 2px #888888;
	}
	label:active{
	   box-shadow:none;
	}
	.modal{
	  display:none;
	  position:fixed;
	  top:0;
	  bottom:0;
	  left:0;
	  right:0;
	  background:rgba(0,0,0,.3);
	}
	#checkbox:checked~.modal{
	  display:flex;
	}
	.close{
	  margin:auto;
	}
	
	```

* CSS position属性的sticky

	盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。在所有情况下（即便被定位元素为 table 时），该元素定位均不对后续元素造成影响。当元素 B 被粘性定位时，后续元素的位置仍按照 B 未定位时的位置来确定。position: sticky 对 table 元素的效果与 position: relative 相同。
	
	粘性定位是相对定位和固定定位的混合，元素在跨越特定阈值前为相对定位，之后为固定定位。例如：
	
	`one { position: sticky; top: 10px; }`
	
	在 viewport 视口滚动到元素 top 距离小于 10px 之前，元素为相对定位。之后，元素将固定在与顶部距离 10px 的位置，直到 viewport 视口回滚到阈值以下。
	
	粘性定位常用于定位字母列表的头部元素。标示 B 部分开始的头部元素在滚动 A 部分时，始终处于 A 的下方。而在开始滚动 B 部分时，B 的头部会固定在屏幕顶部，直到所有 B 的项均完成滚动后，才被 C 的头部替代。
	
	须指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。
	
	```
	<div>
	  <dl>
	    <dt>A</dt>
	    <dd>Andrew W.K.</dd>
	    <dd>Apparat</dd>
	    <dd>Arcade Fire</dd>
	    <dd>At The Drive-In</dd>
	    <dd>Aziz Ansari</dd>
	  </dl>
	  <dl>
	    <dt>C</dt>
	    <dd>Chromeo</dd>
	    <dd>Common</dd>
	    <dd>Converge</dd>
	    <dd>Crystal Castles</dd>
	    <dd>Cursive</dd>
	  </dl>
	  <dl>
	    <dt>E</dt>
	    <dd>Explosions In The Sky</dd>
	  </dl>
	  <dl>
	    <dt>T</dt>
	    <dd>Ted Leo & The Pharmacists</dd>
	    <dd>T-Pain</dd>
	    <dd>Thrice</dd>
	    <dd>TV On The Radio</dd>
	    <dd>Two Gallants</dd>
	  </dl>
	</div>
	
	* {
	  box-sizing: border-box;
	}
	
	dl {
	  margin: 0;
	  padding: 24px 0 0 0;
	}
	
	dt {
	  background: #B8C1C8;
	  border-bottom: 1px solid #989EA4;
	  border-top: 1px solid #717D85;
	  color: #FFF;
	  font: bold 18px/21px Helvetica, Arial, sans-serif;
	  margin: 0;
	  padding: 2px 0 0 12px;
	  position: -webkit-sticky;
	  position: sticky;
	  top: -1px;
	}
	
	dd {
	  font: bold 20px/45px Helvetica, Arial, sans-serif;
	  margin: 0;
	  padding: 0 0 0 12px;
	  white-space: nowrap;
	}
	
	dd + dd {
	  border-top: 1px solid #CCC
	}
	```
	
	![img](../images/sticky.gif)

* font-boosting现象

	font-boosting是指在android chrome对于高度值为auto的字体进行放大的现象，字体越大越明显。一般通过缩放的页面字体都是原来的2至3倍，所以基本上如果字体高度为auto就会触发font-boosting。解决办法给字体一个高度，通过设置height，max-height，line-height即可

* border-radius中50%和100%的区别

	其实border-radius为50%和100%渲染出来的结果是一样的，50%没啥说的，100%的话浏览器渲染左上角的时候，整个会被渲染成一个1/4圆，渲染右上角的时候，如果按照一样的渲染规则，两个相邻圆角合起来就有200%，浏览器自然不会允许这种事情发生，于是浏览器会重新计算。两个圆角的半径都从100%变成了50%~

	具体细节可以参考这篇知乎文章[border-radius的工作原理](https://zhuanlan.zhihu.com/p/20128284)

* 当在移动端页面上覆盖一层宽度和高度为屏幕宽高，position为fixed的图片的时候，页面还是可以滑动的，如果需要在这种条件下禁止页面滑动的话，可以在html标签上设置overflow: hidden

* 记录一下box-shadow的每个参数的含义

	box-shadow: h-shadow v-shadow blur spread color inset;
	
	* h-shadow: 必需，水平阴影的位置，允许负值，正数阴影偏右，负数阴影偏左
	* v-shadow: 必需，垂直阴影的位置，允许负值，正数阴影偏下，负数阴影偏上
	* blur    : 可选，模糊距离
	* spread  : 可选，阴影的尺寸
	* color   : 可选，阴影的颜色
	* inset   : 可选，将外部的阴影改为内部阴影

* flex: auto，flex: 1，flex: none以及flex的默认值

	* flex的默认值为     0 1 auto
	* flex: none         0 0 auto
	* flex: 1            1 1 0
	* flex: auto         1 1 auto

* 水平垂直居中的各种方法(常用)[参考](https://github.com/yanhaijing/vertical-center)

	* 定宽高(假设居中块的长度和宽度为50px)
		
		* absolute + 负margin
			
			```css
			position: absolute;
			left: 50%;
			top: 50%;
			margin-left: -50px;
			margin-right: -50px;
			```
		* absolute + calc

			```css
			position: absolute;
			left: calc(50% - 50px);
			top: calc(50% - 50px);
			```
		
	* 不定宽高

		* absolute + translate

			```css
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translate(-50%, -50%);
			```
		* absolute + margin: auto

			```css
			position: absolute;
			margin: auto;
			left: 0;
			right: 0;
			top: 0;
			bottom: 0;
			```
		
		* flex

			```css
			display: flex;
			justify-content: center;
			align-items: center;
			```
		
		* grid

			```css
			display: grid;
			justify-items: center;
			align-items: center;
			```
		
		* text-align:center + lineheight

			```css
			line-height: 200px;
			text-align: center;
			```
			
		* table-cell

			```css
			display: table-cell;
			vertical-align: center;
			text-align: center;
			```

* 渐变文字的实现，可以用于文本和iconfont

    ```css
    background: linear-gradient(to right, #FF6600 0%, #FF003B 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    ```

* 渐变边框可以用border-image去做，具体可以参考张鑫旭大神的这篇blog[border-image的使用](http://www.zhangxinxu.com/wordpress/2010/01/css3-border-image/)，但是border-image和border-radius并不能同时使用，但是我们可以用伪类层叠的方式来hack一下，具体方法如下

    ```css
    .btn {
        position: relative;
        width: 64px;
        height: 28px;
        border: none;
        color: red;
        background: linear-gradient(to right, #FF6600 0%, #FF003B 100%);
        border-radius: 50px;
        span {
            position: relative;
            z-index: 50; //下载进度不能被遮挡
        }
    }
    .btn::after {
        content: '';
        position: absolute;
        border-radius: 50px;
        background: #FFF;
        width: 62px;
        height: 26px;
        left: 1px;
        top: 1px;
    }
    ```

    将background用渐变色填充，然后用白色背景伪类覆盖中心部分，留出四边相应宽度的边框，hack一个带有圆角的border-image

* 使用3D硬件加速提升动画性能时，最好给元素增加一个z-index属性，人为干扰复合层的排序，可以有效减少chrome创建不必要的复合层，提升渲染性能，移动端优化效果尤为明显[参考资料](https://juejin.im/entry/5adec11151882567236e64ee?utm_source=gold_browser_extension)

* css控制英文、中文强制换行与不换行的代码

    ```css
    word-break: break-all; // 只对英文起作用，以字母作为换行依据
    word-wrap: break-word; // 只对英文起作用，以单词作为换行依据
    white-space: nowrap; // 强制不换行，强制换行
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis; // 不换行，超出部分隐藏且以省略号形式出现
    ```

* background中background-attachment、background-position、background-origin的区别

    * background-attachment: 当指定background-image的时候，该属性决定背景是在视口中固定的还是随包含它的区块滚动的

        * fixed: 此关键字表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着内容滚动
        * local: 此关键字表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动， 并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框
        * scroll: 此关键字表示背景相对于元素本身固定， 而不是随着它的内容滚动（对元素边框是有效的）

    * background-position: 对于每一个被设定的背景图片来说，background-position这个CSS属性设置了一个初始位置。这个初始位置是相对以background-origin定义的背景位置图层（padding-box|border-box|content-box）来说的

    * background-origin: background-origin规定了指定背景图片background-image属性的原点位置的背景相对区域

* ios的webview如何阻止"橡皮筋效果"
    
    [iOS safari 如何阻止"橡皮筋效果"](https://www.zhihu.com/question/22256539)

* 微信游戏上拉显示小图，下拉显示大图的效果如何做，下面给出一个demo

    ```
    <!DOCTYPE html>
	<html>
	<head>
	    <meta charset="utf-8"/>
	    <title>weixin_demo</title>
	    <style type="text/css">
	        body {
	            margin: 0;
	        }
	        .container {
	            overflow: hidden;
	        }
	        .content-wrapper {
	            transform: translateY(-300px);
	            transition: transform .3s ease-out;
	        }
	        .big-banner {
	            transform: translateY(0);
	        }
	        .top-banner {
	            position: relative;
	        }
	        .big-img {
	            background: url('https://sf3-ttcdn-tos.pstatp.com/img/game-files/57d0208992c5a72d8e256a36acb45bc1.jpeg~750x0_q100.webp') no-repeat;
	            background-size: 100% 100%;
	            width: 100%;
	            height: 500px;
	            opacity: 0;
	            transition: opacity .3s ease-out;
	        }
	        .big-img-show {
	            opacity: 1;
	        }
	        .small-img {
	            position: absolute;
	            bottom: 0;
	            background: url('https://sf3-ttcdn-tos.pstatp.com/img/game-files/64869ae5535eb8d0c64d3bf1c078e788.jpeg~cs_670x250_q100.webp') no-repeat;
	            background-size: 100% 100%;
	            width: 100%;
	            height: 200px;
	            opacity: 1;
	            transition: opacity .3s ease-out;
	        }
	        .small-img-hidden {
	            opacity: 0;
	        }
	    </style>
	</head>
	<body>
	    <div class="container">
	        <div class="content-wrapper">
	            <div class="top-banner">
	                <div class="big-img"></div>
	                <div class="small-img"></div>
	            </div>
	            <div class="content">
	                中文名 斗破苍穹 外文名 Fights Break Sphere 出品时间 2017年 出品公司万达影视传媒有限公司、新丽电视文化投资有限公司 制片地区 中国大陆 拍摄地点 云南、都匀影视城 首播时间 2018年9月3日 导    演于荣光 编    剧张挺 主    演吴磊，林允，李沁，陈楚河，刘美彤，谷嘉诚，伍嘉成，肖战，彭楚粤，陈泽希，苏青，凌潇肃，李若彤 集    数 45集 [2]  每集长度 45分钟 类    型 古装、武侠 在线播放平台 腾讯视频 播出平台 湖南卫视 播出剧场 青春进行时
	                剧情简介编辑
	                被视为天赋异禀的少年武者萧炎（吴磊饰），九岁时母亲古文心（李若彤饰）被族人强行带回家族囚禁，父亲萧战（于荣光饰）讳莫如深。萧炎在练功时捡到一枚戒指，但是却令萧炎的功力减弱无法修炼，早已订好婚约的家族也来退婚，令萧家蒙受奇耻大辱。萧炎无意中唤醒了戒指的主人药尘（陈楚河饰）老人，在药尘老人的帮助下，萧炎的武功进步惊人，并且得知了母亲的下落。萧炎进入迦南学院学习艺技，在这里结交了一群良师益友，再次引起了敌人的注意。一次特殊修炼中，萧炎遭到陷害，死里逃生后，发现他的家族也遭到了灭顶之灾。为了世间和平，萧炎毅然选择了只身一人向邪恶的势力发起挑战。最后联手女侠萧薰儿（林允饰），共同战胜了强敌 [2]  。
	
	
	                分集剧情编辑
	                第1集
	                　　一千年前，被誉为‘大陆太阳’的斗帝率领五大家族对抗北方极寒之地的魂殿，将他们封印在黑暗的地下，为斗气大陆带来和平。眼看着日夜交替，光明就要陨落，黑暗即将崛起，五大家族子弟坚定地守卫着这份光明，与魂殿迎来一战。乌坦城，五大家族的萧家之子萧炎一表人才，天资聪颖，九岁时便练成了九段斗气，是大陆最强的天才少年，曾获得少年斗气大会的冠军，一举成名。同时也是那一年，北方传来了黑暗复苏的消息，他的母亲参加了五大家族远赴魂殿的北征。北征回来后，萧母身负重伤，将一枚纳戒交给了萧炎，让萧炎好生保管。除此外，其余几大家族皆前来向萧母质问星陨阁宝藏的下落，认定是萧母背叛了五大家族，投靠了魂殿且盗走宝藏。萧母称自己并不知宝藏下落，她将魂殿的主人魂灭生的话带给了另一家族云岚宗的宗主，云宗主听后心生畏惧，萧母则当场自杀。萧母自杀并未了结此事，其余四大家族继续向萧父萧战施压，逼他交出宝藏，萧战被逼无奈，只好自废斗气，以证清白，萧家也从此没落。萧炎七年来一直苦练斗气，他想努力做一个强者，查清母亲逝去的真相，可七年来不管他如何努力，他的斗气却一直在退步，成为了斗气大陆上最后一个废柴，唯一一个相信他，对他不离不弃的是从小和他一起长大的青梅竹马萧熏儿。萧熏儿始终陪伴在萧炎的身边，可萧家斗技馆中的众师兄弟却对萧炎冷嘲热讽，认定他就是废物一个。萧炎不服众人的奚落嘲讽，他回到家中希望父亲能够传他斗气功法，以此打败所有看不起他的人，萧战不愿意传授其功法，并称萧炎并不是那块料。萧炎少年好胜心强，他想要凭借着自己的努力改变所有人对他的看法，不愿意像自己的父亲一样自废斗气躲在家中看账本，萧炎的一番话激起了萧战的怒气，引来了萧战的一顿责罚跟批评。斗技馆中，萧炎一直不放弃不服输地练习着功法，萧熏儿对萧炎寄予厚望，为他加油打气。这时，萧战命人传萧炎回府，他请来了药铺的掌柜万先生，希望萧炎能够放弃斗气，拜万先生为师，成为药铺伙计。萧炎心心念念是想考进斗气大陆的最高学府——迦南学院成为一名斗者，可萧战却提起了萧炎在今年斗之气大会中的落后排名，打击着萧炎的信心，称以萧炎这样的资质成为斗者只会轻易送命，他希望萧炎能够认命，安分学习药材知识，可萧炎天生性子倔强，不肯服输跟认命，萧战无奈，只好代萧炎向万先生行拜师大礼，奉上热茶，强迫萧炎成为万先生的徒弟，严禁萧炎再踏进斗技馆一步。药铺中，药铺伙计教萧炎认药材并吩咐他将药材送往斗技馆，萧炎不甘心自己的现状，他到野外发泄着脾气却误打误撞进入了一个异常空间，他在空间中遇到一个紫发蓝衫的男人，男人自称是他师爷爷。萧炎并不相信，他的师爷爷药尘早在七年前便已经死亡，药尘也没有瞒着萧炎，他称七年前他便将自己同星陨阁的宝藏一同封印在了萧炎手戴的纳戒空间里，当年是萧母拼死救出了他，可他的两个徒弟一个自杀身亡，一个投靠魂殿，他迫不得已只能一个人孤独地守在这里，靠吸食萧炎的斗之气活到现在，这也是萧炎七年来天天练斗气天天退步的原因。萧炎得知自己斗气倒退的原因后愤怒不已，若不是因为药尘的吸食，他也不会受了这么多侮辱，他对药尘大打出手，却不是药尘的对手。得知了药尘的厉害之处，萧炎希望药尘能够跟他出去，替他报了萧母的仇，药尘拒绝了萧炎的提议并将萧炎踢出了纳戒。
	
	                第2集
	                　　萧炎当着纳兰家族的面吃下了婚书，纳兰家族方才罢休，离开了萧家。萧家受到前所未有的侮辱，萧炎孤身离开大堂，他向自己的父亲表明，他一定会让纳兰家族偿还今天的所有侮辱。大堂外，纳兰嫣然将药物送给萧炎，萧炎看到药物上的佩坠，不由得想起初次见纳兰嫣然的场景，当时他还是人尽皆知的斗气天才，纳兰嫣然对他的态度也并非如此，萧炎没有接过嫣然的药，直接从她的面前离开。不远处的萧熏儿不满嫣然对萧炎的态度，她站出替萧炎说话，称萧炎既然能在七年之前创造奇迹，也必定能在以后出人头地，届时嫣然定会后悔今日的决定。夜晚，萧熏儿前来给萧炎送药，她替萧炎打抱不平，并想在药铺学打算盘陪着萧炎，萧炎不知道萧熏儿的女儿家心思，只让萧熏儿趁早打消这个念头。待萧熏儿离开后，萧炎用酒诱惑着药尘现身，再次进入了纳戒的空间中。药尘问起萧炎现如今的能力，萧炎坦言称他的斗气只有三段，药尘听此，毫不留情地称三年后萧炎根本不会是嫣然的对手，嫣然早已拜云山当了老师，三年后的实力必定突飞猛进。提起嫣然，萧炎径自对自己的订婚宴进行一番嘲讽，准备苦练斗气。药尘也亲自用异火跟药材炼出了能够迅速提升斗气段位的筑基灵液，让萧炎拿出去卖个好价钱，给他换酒。萧炎在院中苦练功夫，萧战看到萧炎的努力还是深叹了一口气，他带着萧炎来到魂殿安插在萧家的内奸面前，内奸已经死亡，萧战认为除了萧家之外，其他家族也必定存在着魂殿的内奸，魂殿若是想要对五大家族逐个击破瓦解，也并非难事。随后，萧战拿出了自己在市场上高价购回的筑基灵液，称这一瓶筑基灵液能够帮助萧炎提升两个段位，他告诫萧炎，修行就像爬山一样，有高有低，他对萧炎进行一番鼓励并默许他学斗之气，萧炎想进斗技馆学习，可萧父却认为萧炎藏身于药铺会比较安全。纳戒里，萧炎跟药尘促膝而谈，药尘提起了萧炎母亲的事情。其实，萧家跟星陨阁向来关系交好，但后来两家翻脸跟萧炎脱不了干系。萧母个性分明，她嫁给萧战之后就不继续留在星陨阁学习。萧母是药尘最深爱的弟子，也是他亏欠最多的人，药尘认为萧母临走前一定会很埋怨他，可萧炎却称萧母从来没有埋怨过药尘，萧母临终前走得很平静，而他也会完成萧母对他的嘱托，拼上性命保护好药尘，守护好纳戒，即便药尘不愿意教他学斗气。萧炎学斗气的想法依旧坚定不移，他不希望自己像药尘一样，虽然成为了天下第一的炼药师，最终却自暴自弃，什么都躲避着。药尘被萧炎的这番话所激中，他提起了纳戒空间内的二阶魔龙斗蝎，要求萧炎独自挑战龙蝎，如果他挑战成功，他便会教萧炎斗气，但如果萧炎因害怕而出声求救，日后就不得再提斗气一事。
	            </div>
	        </div>
	    </div>
	    <script type="text/javascript">
	        const container = document.getElementsByClassName('container')[0];
	        const wrapper = document.getElementsByClassName('content-wrapper')[0];
	        const bigImg = document.getElementsByClassName('big-img')[0];
	        const smallImg = document.getElementsByClassName('small-img')[0];
	        let startY = null;
	        let endY = null;
	
	        const blockScroll = (ev) => {
	            if (event.cancelable) {
	                ev.preventDefault();
	                ev.stopPropagation();
	                setTimeout(() => {
	                    document.removeEventListener('touchmove', blockScroll, {
	                        capture: false,
	                        passive: false,
	                    })
	                }, 300);
	            }
	        }
	
	        container.addEventListener('touchstart', (ev) => {
	            if (document.documentElement.scrollTop === 0) {
	                startY = ev.targetTouches[0].pageY;
	            }
	        });
	        container.addEventListener('touchend', (ev) => {
	            if (startY !== null) {
	                endY = ev.changedTouches[0].pageY;
	                const distance = Math.abs(endY - startY);
	                if (distance < 10) return;
	                // 上滑
	                if (startY > endY) {
	                    wrapper.classList.remove('big-banner');
	                    bigImg.classList.remove('big-img-show');
	                    smallImg.classList.remove('small-img-hidden');
	                } else { // 下滑
	                    wrapper.classList.add('big-banner');
	                    bigImg.classList.add('big-img-show');
	                    smallImg.classList.add('small-img-hidden');
	                }
	                document.addEventListener('touchmove', blockScroll, {
	                    capture: false,
	                    passive: false,
	                });
	                startY = null;
	            }
	        })
	    </script>
	</body>
	</html>
    ```
