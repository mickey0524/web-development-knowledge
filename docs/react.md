<h2 id="react">React</h2>

* 在react虚拟内存中获取DOM元素的方法

	原先的this.refs.nodeName会有问题，现在官方提供了回调函数的新用法，未来可能会把refs这种旧的API下掉

	`<input type="text" ref={(input) => { this.textInput = input; }}  />`

	通过这种方法，就把该DOM元素绑定在了this.textInput上23333

* React中类型检查的方法

	Vue中可以直接在props中进行类型检查，React中自然也有相应的类型检查方法，但需要注意的是，React.PropTypes自React v15.5起已弃用，请使用prop-types库代替
	
	```
	import PropTypes from 'prop-types';
	
	class Greeting extends React.Component {
	  render() {
	    return (
	      <h1>Hello, {this.props.name}</h1>
	    );
	  }
	}
	
	Greeting.propTypes = {
	  name: PropTypes.string
	};
	```

* React使用webpack打包的时候，需要使用babel-loader，babel-loader需要配置一下
	
	`"presets": ["es2015", "react", "stage-2"],`这是babelrc中的配置
	
	前端想用es6的import from需要配置babel，但是webpack2之后能够识别import，所以配置了babelrc很多时候是没有用的，但是node-server和react一定要记得配置

* React中类似Vue v-html 的方法

	`<div dangerouslySetInnerHTML={{__html: this.state.info}} />`

* React中State的更新可能是异步的，因此不应该直接依赖state和props来更新现有的state，可以使用如下方法：

	```js
	this.setState((preState, props) => {
		counter: preState.counter + props.increment
	});
	```

* React中可以通过render函数返回null来hide自身

* React在map函数中调用组件(例如ListItem)，key属性需要定义到ListItem上，但是需要注意的是，key属性不会作为props的属性传递给子组件，贼坑，如果在子组件里需要用到key，需要显示的传入另外一个prop~

* React的包含关系（类似于Vue中的slot属性）
	
	一些组件不能提前知道它们的子组件是啥，对于这种组件，可以使用`children`属性将子元素直接传递到输出
	
	```
	function FancyBorder(props) {
 	  return (
   	    <div className={'FancyBorder FancyBorder-' + props.color}>
     	      {props.children}
   	    </div>
 	  );
	}
	
	function SplitPane(props) {
 	  return (
   	    <div className="SplitPane">
     	      <div className="SplitPane-left">
       	        {props.left}
     	      </div>
     	      <div className="SplitPane-right">
       	        {props.right}
     	      </div>
   	     </div>
 	  );
	}
		
	function App() {
 	  return (
   	    <SplitPane
     	      left={
       	        <Contacts />
     	      }
     	      right={
       	        <Chat />
     	    } />
 	  );
	}	
	```

* 从本质上来讲，JSX只是为React.createElement(component, props, ...children)方法提供的语法糖

	```
	<MyButton color="blue" shadowSize={2}>
	  Click Me
	</MyButton>
	```

	会被编译为

	```
	React.createElement(
	  MyButton,
	  { color: 'blue', shadowSize: 2 },
	  'Click Me'
	)
	```

* React在ES5中通过在React.createClass()函数中的getDefaultProps来给props设置默认数值，在ES6中，通过 class MyComponent extends React.Component {} 来创建组件，然后通过 MyComponent.defaultProps = { name: 'stranger' } 来为props设置默认值。

* React中可以通过ref回调函数的传递来使得父亲组件直接操作儿子组件的DOM元素，如下所示：

	```
	function CustomTextInput(props) {
 	  return (
   	    <div>
     	      <input ref={props.inputRef} />
   	    </div>
  	  );
	}

	function Parent(props) {
	  return (
   	    <div>
     	      My input: <CustomTextInput inputRef={props.inputRef} />
   	    </div>
 	  );
	}

	class Grandparent extends React.Component {
  	  render() {
   	    return (
     	      <Parent
                inputRef={el => this.inputElement = el}
     	      />
   	    );
 	  }
	}
	```

	上面的例子中，Grandparent 首先指定了 ref 回调函数。它通过一个常规的 inputRef 属性被传递到 Parent，Parent 也同样把它传递给了 CustomTextInput。最后 CustomTextInput 读取了 inputRef 属性并将传递的函数作为 ref 属性附加到 \<input>。最终，Grandparent 中的 this.inputElement 被设置为 CustomTextInput 的 input 对应的 DOM 节点.

* React中的HOC

	[React中的高阶组件](https://doc.react-china.org/docs/higher-order-components.html)

* es5中createClass自动给其中的方法绑定了this，在es6的class方式定义中，需要在constructor中手动绑定this

	```
	import React, { Component } from 'react';
	
	class MyComponent extends Component {
		constructor(props) {
			super(props);
			this.onClickHandler = this.onClickHandler.bind(this);
		}
		
		onClickHandler() {
			...
		}
	}
	```

* RN可以作为submodule被客户端的代码仓库依赖，也可以上传CDN，然后客户端创建一个RN的环境，直接执行js代码

* react在写组件的时候可以 extends Component 或者是 extends PureComponent，PureComponent只会对
state中的数据进行浅层次的数据比较，能够大大提高update view 的效率，当需要深层比较的时候，就只能extends Component了