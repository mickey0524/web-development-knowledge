* 移动端有关touch的事件

	touchstart, touchmove, touchend(移动端监听用户手势事件)

* dpr和移动端适配

	http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html

	一般计算dpr和font-size都是动态在html上使用dataset去设定的

* 网上都说操作真实 DOM 慢，但测试结果却比 React 更快，为什么？

	参照知乎yyx大大的回复 https://www.zhihu.com/question/31809713/answer/53544875

* android机器经常不能高度居中，但是IOS和chrome devtool明明是居中的，是不是很痛苦？
	
	首先，尽量不要使用奇数的px，android对小数不敏感，其次，由于IFC的原因，不同android height和line-height的基准是不一样的，可以试着把height去掉，让line-height自动撑起高度，这样有助于在android上实现高度居中

* 移动端不能识别 $('html, body').scrollTop, 单纯就是因为moblie ios 不认识$(‘html,body’)，改成$('body')就好

	http://blog.jonathanargentiero.com/jquery-scrolltop-not-working-on-mobile-devices-iphone-ipad-android-phones/

* 响应式布局（类似flexible的rem计算方式）手机字体大小会影响app中的webview元素的大小

	这个简直是天坑，和组内的大佬找了2个多小时吧，最后定位到是手机字体的大小会影响webview元素的大小，大部分手机的默认字体大小是16px，这个大小不会有问题，三星默认大小比16px大一号，结果计算出来的元素均会偏大，难受的一匹，解决方法是可以让客户端将字体大小写死为16px，允悲	

* Android浏览器文本垂直居中问题

	android浏览器上下居中始终存在问题，苹果浏览器是没有问题的，解决问题的方法是使用table布局

	```
	<div class="container">
		<div class="sub-container">居中</div>
	</div>
	
	.container {
		display: table;
	        width: 100px;
	        height: 100px;
	}
	.sub-container {
	        text-align: center;
	        display: table-cell;
	        width: 40px;
	        height: 40px;
	        vertical-align: middle;
	}
	```

* CSS3 动画在 iOS 上为什么会因为页面滚动也停止

	最近在做移动端的feed流的时候，发现一个现象，当手指按住屏幕，触发touch事件的时候，ios手机是不会响应任何操作的，包括（接口，动画...），当时百思不得其解，以为是bug，后来发现，ios为了流畅度，将touch事件放在监听的第一位，详情可以参考下面的连接

[CSS3动画在ios上为什么会因为页面滚动而停止](https://www.zhihu.com/question/24268253)

* 移动端监听软键盘的"完成"按钮

	* 模拟表单提交，在input元素外面包裹一层form元素，然后监听form元素的submit事件
	* 监听input元素的keypress事件，ev.charCode == 13的时候表示按下了完成按钮

* 移动端实时监听input元素的输入事件不能使用change事件，要监听input事件

* 移动端 js input 事件输入汉字的时候，拼音在输入框内也会触发input事件，可以用compositionstart和compositionend两个事件形成一个业务锁，避免发送无效的请求

    ```js
    this.searchInput.on('input', () => {
      if (this.lock) return; // 中文输入中间的input事件过滤
      this.fetchSearchRes();
    });
    this.searchInput.on('compositionstart', () => { // 开始中文打字，上锁
      this.lock = true;
    });
    this.searchInput.on('compositionend', () => { // 中文选取完毕，解锁
      this.lock = false;
      this.fetchSearchRes();
    });
    ```

* 移动端经常会有这样的场景，底部要fixed一个input框，这个时候不用使用`position: fixed`，input会被弹起的键盘遮盖，可以使用`position: absolute`，另外还有一个api`scrollIntoView`，用法如下，这个api还可以设置对齐方法，选择将input放在屏幕的上方/下方，类似的api还有`Element.scrollIntoViewIfNeeded`

    ```js
    const input = document.querySelector('#inputId');
    input.scrollIntoView();
    ```
* 移动端JSBridge为啥先发一个dispatch\_message通知客户端，然后客户端调用js里的`_fetchQueue`方法，`_fetchQueue`里再将具体的参数发送给客户端

    JSBridge调用端上提供的方法其实是通过改变iframe的src来发dispatch和发真正的事件的，客户端通过拦截URL请求来监听，如果是一次JSBridge事件一跳dispatch带事件参数过去的话，会有一个问题，假设短时间内多次触发JSBridge调用，相当于是频繁改变src，这个时候，前几次的src改变完可能请求还没发出去，就被后面的覆盖了，简单来说，就是丢了事件，因此搞成这种两跳的，事件先推队列，触发dispatch message，只要随便有一个dispatch message被监听到，就fetchQueue取出整个队列通过src送回去，客户端拿到这个fetch的结果就for循环挨个执行，后面再跑过来的dispatch取出来是空队列，就丢弃掉。