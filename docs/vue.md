* 修改Vue对象数组中数组，view层不生效，原因是没有使用$set，直接修改并不会给你重新渲染

    ```js
    this.$set(array/object, index/key, newVal);
    Vue.$set(array/object, index/key, newVal);
    ```

* 修改Vue对象数组的元素的一个字段，直接赋值是不行的，Object.assign(原对象，{ key: value })也是不行的，因为这个操作默认是浅拷贝

	使用Object.assign({}, 原对象, { key: value })是可以的

* Vue中，尽量不要使用箭头函数，像methods中的方法，watch中的方法，箭头函数绑定了父级作用域的上下文，所以this不会按照期望指向Vue实例

* Vue的生命周期简介

	Vue在初始化组件数据的时候，在生命周期的beforeCreate和created钩子函数之间实现了对data、props、computed、methods、events以及watch的处理，在渡过created生命周期之后，Vue会去判断options对象中是否包括el这个key，如果不存在就要等待instance.$mount()手动进行挂载(instance 是由extend创建的构造器实例化出来的实例)，紧接着，Vue会去判断是否存在template或者render，存在的话，用template进行编译，反之，将el的outerHTML作为template进行编译，然后就是mounted()进行挂载... 

   * beforeCreate：组件实例刚刚被创建，组件属性计算之前，如data属性
   * created：组件实例创建完成，属性已绑定，但是DOM还未完成，$el属性还不存在
   * beforeMount: 模版编译/挂载之前
   * mounted: 模版编译/挂载之后
   * beforeUpdate: 组件更新之前
   * updated: 组件更新之后
   * activated: for `keep-alive`，组件被激活时调用
   * deactivated: for `keep-alive`，组件被移除时调用
   * beforeDestory: 组件销毁前被调用
   * destoryed: 组件销毁后调用

* 计算属性的setter

	计算属性默认只有getter，不过在需要时你也可以提供一个setter:

	```js
	computed: {
 		fullName: {
   			 // getter
   			 get: function () {
     				 return this.firstName + ' ' + this.lastName
   			 },
   			 // setter
   			 set: function (newValue) {
     				 var names = newValue.split(' ')
     				 this.firstName = names[0]
     				 this.lastName = names[names.length - 1]
   			 }
 		 }
	}
	```
	
	注意这里，Vue中是可以通过修改计算属性，以及设置set来反向修改计算属性依赖的data

* vue中，在计算属性能够解决的问题作用范围内，尽量使用计算属性，但是需要注意的一点是，计算属性是同步的，有时候需要异步的修改，这个时候可以使用watch。

* vue中，transition不止可以用来设置过渡动画，还可以提供钩子函数，用于执行一些操作

	[Vue的transition钩子函数](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-钩子)

* vue在处理生命周期的lifecycle.js(src/core/instance/lifecycle.js)中定义了一个callHook函数

	```
	export function callHook (vm: Component, hook: string) {
	  const handlers = vm.$options[hook]
	  if (handlers) {
	    for (let i = 0, j = handlers.length; i < j; i++) {
	      try {
	        handlers[i].call(vm)
	      } catch (e) {
	        handleError(e, vm, `${hook} hook`)
	      }
	    }
	  }
	  if (vm._hasHookEvent) {
	    vm.$emit('hook:' + hook)
	  }
	}
	```

	从上面vue的源码可以看出，vue在每个生命周期都会emit一个事件，我们可以通过$on('hook:mounted')之类的语句来监听vue的生命周期，这对于自定义指令很有用，这里有一篇讲的不错的博客[理解vue的生命周期钩子](https://segmentfault.com/a/1190000010006604)

* new Vue(option)中，option中有一个key为replace，指代是否要使用template去替换el元素，如果不加这个key，template替换了el，让组件暴露在最顶层，可能会报下面的错

	```
	[Vue warn]: Attribute "id" is ignored on component <div> because the component is a fragment instance
	```

* vue中props如果要设置default为array或者object，需要用function返回

* vue中，除了顶层入口文件data可以直接返回object，其他子组件的data需要返回一个function，原因是object是引用数据类型，每个组件的data如果直接返回object的话，则组件的多个实例共享一个数据对象，这显然是不符合预期的，于是需要用一个function来返回

* element针对vue2实现的broadcast和dispatch

	核心思想是基于mixins(不知道mixin的童鞋可以移步这里[mixin](https://vuefe.cn/v2/guide/mixins.html))通过$parent和$children遍历虚拟dom树，比对componentName和vm.$option.componentName，成功的话，触发$emit方法，源码如下

	```js
	function broadcast(componentName, eventName, params) {
  /*遍历当前节点下的所有子组件*/
		this.$children.forEach(child => {
			/*获取子组件名称*/
			var name = child.$options.componentName;

			if (name === componentName) {
				/*如果是我们需要广播到的子组件的时候调用$emit触发所需事件，在子组件中用$on监听*/
				child.$emit.apply(child, [eventName].concat(params));
			} else {
				/*非所需子组件则递归遍历深层次子组件*/
				broadcast.apply(child, [componentName, eventName].concat([params]));
			}
		});
	}
	export default {
		methods: {
			/*对多级父组件进行事件派发*/
			dispatch(componentName, eventName, params) {
				/*获取父组件，如果以及是根组件，则是$root*/
				var parent = this.$parent || this.$root;
				/*获取父节点的组件名*/
				var name = parent.$options.componentName;

				while (parent && (!name || name !== componentName)) {
					/*当父组件不是所需组件时继续向上寻找*/
					parent = parent.$parent;

					if (parent) {
						name = parent.$options.componentName;
					}
				}
				/*找到所需组件后调用$emit触发当前事件*/
				if (parent) {
					parent.$emit.apply(parent, [eventName].concat(params));
				}
			},
			/*
					向所有子组件进行事件广播。
					这里包了一层，为了修改broadcast的this对象为当前Vue实例
			*/
			broadcast(componentName, eventName, params) {
				broadcast.call(this, componentName, eventName, params);
			}
		}
	};
	```

	如果要多发的话，可以将componentName类型由String => Array，如果要全部发送的话，则全部遍历$children和$parent触发$emit即可
	
* vue2中的extends

	类型：`Object | Function`
	
	允许声明扩展另一个组件(可以是一个简单的选项对象或构造函数)，而无需使用`Vue.extend`，这主要是为了便于扩展单文件组件，和`mixins`类似
	
	```js
	var CompA = { ... }

	// 在没有调用 `Vue.extend` 时候继承 CompA
	var CompB = {
	  extends: CompA,
	  ...
	}
	```
	
* vue2中的provide/inject

	类似于react中的context，不推荐直接用于应用程序代码中，这对选项需要一起使用，以允许一个祖先组件向其后代注入一个依赖，不论组件层次有多深，具体见[vue2文档](https://cn.vuejs.org/v2/api/#provide-inject)
	
* vue2中的浏览器环境检查

    ```js
    export const inBrowser = typeof window !== 'undefined'
    export const UA = inBrowser && window.navigator.userAgent.toLowerCase()
    export const isIE = UA && /msie|trident/.test(UA)
    export const isIE9 = UA && UA.indexOf('msie 9.0') > 0
    export const isEdge = UA && UA.indexOf('edge/') > 0
    export const isAndroid = UA && UA.indexOf('android') > 0
    export const isIOS = UA && /iphone|ipad|ipod|ios/.test(UA)
    export const isChrome = UA && /chrome\/\d+/.test(UA) && !isEdge
    ```	

* Vue2中提供了vm.$refs来访问真实节点，如果绑定ref的节点是真实DOM，那么vm.$refs[key]指代的DOM节点，如果绑定ref的是Vue组件，那么vm.$refs[key]指代的是对应组件的componentInstance，源码如下

    ```js
    const key = vnode.data.ref
    const ref = vnode.componentInstance || vnode.elm
    const refs = vm.$refs
    ...
    refs[key] = ref 
    ```

* 记一次使用vue-lazyload插件的hock过程

    当DOM是由display：none变为display: block出现的时候，是不会触发vue-lazyload的视图更新的，查阅了一下，发现vue-lazyload还监听如mousewheel这样的事件，可以在切换的时候，手动触发一个DOM事件，进行hock

    ```js
    const event = document.createEvent('MouseEvents');
    event.initEvent('mouseWheel', true, true);
    document.body.dispatchEvent(event);
    ```

* Vue中的eventbus

    Vue中的eventbus，也可以说是eventhub，在业务逻辑不是特别复杂的时候，在兄弟组件中传递数据不需要使用vuex，使用一个vue实例做桥接就行了，例子如下

    ```js
    const eventBus = new Vue();
    
    // 好的代码风格，设置eventBus实例为只读的
    Object.defineProperty(Vue.prototype, $bus, {
        get: function() {
            return eventBus;
        },
    });

    ...

    // componentA

    this.$bus,$on('login', (eventData) => {
        // pass
    });

    // componentB

    this.$bus.$emit('login', eventData);
    ```

    需要注意的是，如果是在单页应用中使用eventBus，在页面切换的时候，记得要eventBus.$off掉创建的监听器，否则容易触发多次无用callback函数
