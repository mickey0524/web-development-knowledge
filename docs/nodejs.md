
* 都说Node.js是单线程，这是真的吗？

	对，也不对。可以参考浏览器里的script来理解，浏览器端的javascript是单线程的，但是为什么我可以同时发送一堆ajax请求，还能响应DOM事件，是谁在调用这些回调？答案很简单，宿主环境。虽然javascript代码本身是单线程执行的，但是浏览器是多线程的。Node.js也一样，虽然你写的代码是单线程的，但是Node.js本身会有一堆线程干别的事。

* 入坑Node.js的同学多半是听说Node.js高并发、性能好，这是对的吗？

	对，也不对。高并发是对的，性能因为V8的强大，相对于脚本语言（php，python）来说会有优势，但是绝对的性能（C++、Java、Go）肯定谈不上好

* 性能不好怎么还高并发呢，这不是矛盾了吗？

	并不矛盾，高并发并不意味着快，只代表Server能同时接受多少请求，但管杀不管埋，每个请求接进来之后能不能快速的处理掉，还是跟你的代码有关。所以当你的代码是CPU密集的时候，请慎用Node.js。Node.js的优势在与IO密集的场景能更多的压榨单机的性能。

* js 中， 0.1 + 0.2 === 0.3 是否为 true ? 在不知道浮点数位数时应该怎样判断两个浮点数之和与第三数是否相等？

	答案肯定是不为true的，因为Javascript的number类型按照ECMA的Javascript标准，它的Number类型就是IEEE 754的双精度数值，因此Javascript在做四则运算的时候，精度会丧失

	至于后面这个问题，我的话，会采用这个方法
	```
		if (Math.abs(num1 - num2) < 1E-10) {
			// do something
		}
	```

* a.js和b.js两个文件互相require是否会死循环？双方是否能导出变量？如何从设计上避免这种问题？

	不会的，下面给出require的实现机制

    	function require(...) {
    		var module = { exports: {} };
    		((module, exports) => {
    			function some_func() {};
    			exports = some_func;
    			module.exports = some_func;
    		})(module, module.exports);
    		return module.exports;
    	}
	从上面的实现代码也可以看出exports就是module.exports的一个引用，具体这两个之间的关系见下方url https://cnodejs.org/topic/5734017ac3e4ef7657ab1215

* Event是Nodejs中一个非常重要的core模块，在node中有许多重要的core API都是依赖其建立的.比如Stream是基于Events实现的，下面我们来看几段event相关的代码

    	const EventEmitter = require('events');
    
    	let emitter = new EventEmitter();
    	
    	emitter.on('myEvent', () => {
    	  console.log('hi 1');
    	});
    	
    	emitter.on('myEvent', () => {
    	  console.log('hi 2');
    	});
    	
    	emitter.emit('myEvent');

	上方的代码会依次输出hi 1和hi 2

	const EventEmitter = require('events');

    	let emitter = new EventEmitter();
    	
    	emitter.on('myEvent', () => {
    	  console.log('hi');
    	  emitter.emit('myEvent');
    	});
    	
    	emitter.emit('myEvent');

	上方代码会死循环，道理也很简单，第一次监听到了myEvent，然后又发出了myEvent的信号，这个是全局的，继续监听，然后就死循环了

	const EventEmitter = require('events');

    	let emitter = new EventEmitter();
    	
    	emitter.on('myEvent', function sth () {
    	  emitter.on('myEvent', sth);
    	  console.log('hi');
    	});
    	
    	emitter.emit('myEvent');

	这样就不会死循环，因为第二次监听之后，已经没有发出myEvent事件了

* 用js实现一个sleep函数？

	Nodejs中执行js代码的过程是单线程的，只有当前代码都执行完，才会切入事件循环，然后从事件队列中pop出下一个回调函数开始执行代码，所以实现一个sleep函数，只要通过一个死循环就可以阻塞整个js的执行流程
    
    	function sleep(ms) {
    		var start = Date.now(), expire = start + ms;
    		while (Date.now() < expire) { }
    		return ;
    	}

* Child Process中child.kill和child.send的区别

	child.kill是基于信号系统，child.send是基于IPC通道

* 父进程或子进程的死亡是否会影响对方？什么是孤儿进程？

	子进程死亡不会影响父进程，不过子进程死亡时，会向它的父进程发送死亡信号，反之父进程死亡，一般情况下子进程也会随之死亡，但如果此时子进程处于可运行状态的话，子进程将被init进程收养，从而成为孤儿进程，另外，子进程死亡的时候，父进程没有及时调用wait()或者waitpid()来返回死亡进程的相关信息，此时子进程还有一个PCB残留在进程表中，被称为僵尸进程

* 初识cluster

	Cluster是最常见的Node.js利用多核的办法，它是基于child_process.fork()来实现的，所以cluster产生的进程之间是通过IPC来通信的，并且它也没有拷贝父亲进程的空间，而是通过加入cluster.isMaster这个标识来区分父亲进程还是子进程

    	const cluster = require('cluster');
    	const http = require('http');
    	const numCPUs = require('os').cpus().length;
    
    	if (cluster.isMaster) {	
    		for (var i = 0; i < numCPUs; i++)
    			cluster.fork();
    		}
    		cluster.on('exit', (worker) => {
    			console.log(`${worker.process.pid} died`);
    		});
    	}
    	else {
    		http.createServer((req, res) => {
    			res.writeHead(200);
    			res.end('hello world\n');
    		}).listen(8000);
	在上述代码中 numCPUs 虽然是全局变量但是, 在父进程中修改它, 子进程中并不会改变, 因为父进程与子进程是完全独立的两个空间. 他们所谓的共有仅仅只是都执行了, 并不是同一份.
	
	你可以把父进程执行的部分当做 a.js, 子进程执行的部分当做 b.js, 你可以把他们想象成是先执行了 node a.js 然后 cluster.fork 了几次, 就执行执行了几次 node b.js. 而 cluster 模块则是二者之间的一个桥梁, 你可以通过 cluster 提供的方法, 让其二者之间进行沟通交流.

	一般都会把处理逻辑封装成一个app.js，那么在else里面require('./app.js')是个不错的选择

	最后大力推荐一下pm2管理，pm2 start app.js -i 4 起4个工作进程

* 父进程和子进程之间的IPC通道是怎么建立的?
	
	在通过 `child_process` 建立子进程的时候，是可以指定子进程的env(环境变量)的，所以Node.js在启动子进程的时候，主进程会先建立IPC通道，然后将IPC通道的fd(文件描述符)通过环境变量(NODE_CHANNEL_FD)的方式传递给子进程，然后子进程通过fd连上IPC与父进程建立连接

* 如何创建一个Buffer对象，知道new Buffer()被逐渐废弃了么，知道为什么吗？

	ps: 对buffer对象使用typeof得到的是object

	在Node.js v6.x之前，我们创建buffer对象使用的都是new Buffer()的方法，但是在此之后，该方法开始被废弃，理由是参数类型不同会返回不同类型的Buffer对象，所以当开发者没有正确校验参数或没有正确初始化Buffer对象的内容时，以及不了解的情况下初始化就会在不经意间向代码中引入安全性和可靠性问题。

	因此当前的方法是

	`Buffer.from()` 根据已有数据生成一个Buffer对象

	`Buffer.alloc(size)` 创建一个大小为size的Buffer对象

* 自己实现一个console.log()

    	let print = (str) => process.stdout.write(str + '\n');
    	
    	print('hello world');

* Nodejs如何同步的获取用户的输入

	在Nodejs中，输入流 (stdin), 输出流 (stdout), 错误流 (stderr) 三者. 在 Node.js 中分别对应 process.stdin (Readable), process.stdout (Writable) 以及 process.stderr (Writable) 三个 stream.

	下面给出stackoverflow上的实现方法

	    /*
    	 * http://stackoverflow.com/questions/3430939/node-js-readsync-from-stdin
    	 * @mklement0
    	 */
    	var fs = require('fs');
    	
    	var BUFSIZE = 256;
    	var buf = new Buffer(BUFSIZE);
    	var bytesRead;
    	
    	module.exports = function() {
    	  var fd = ('win32' === process.platform) ? process.stdin.fd : fs.openSync('/dev/stdin', 'rs');
    	  bytesRead = 0;
    	
    	  try {
    	bytesRead = fs.readSync(fd, buf, 0, BUFSIZE);
    	  } catch (e) {
    	if (e.code === 'EAGAIN') { // 'resource temporarily unavailable'
    	  // Happens on OS X 10.8.3 (not Windows 7!), if there's no
    	  // stdin input - typically when invoking a script without any
    	  // input (for interactive stdin input).
    	  // If you were to just continue, you'd create a tight loop.
    	  console.error('ERROR: interactive stdin input not supported.');
    	  process.exit(1);
    	} else if (e.code === 'EOF') {
    	  // Happens on Windows 7, but not OS X 10.8.3:
    	  // simply signals the end of *piped* stdin input.
    	  return '';
    	}
    	throw e; // unexpected exception
    	  }
    	
    	  if (bytesRead === 0) {
    	// No more stdin input available.
    	// OS X 10.8.3: regardless of input method, this is how the end 
    	//   of input is signaled.
    	// Windows 7: this is how the end of input is signaled for
    	//   *interactive* stdin input.
    	return '';
    	  }
    	  // Process the chunk read.
    	
    	  var content = buf.toString(null, 0, bytesRead - 1);
    	
    	  return content;
    	};

* path模块中path.resolve()和path.join()的区别

	在很多环境下，path.resolve()和path.join()会返回相同的数值，看一看下面这个例子

	```js
	path.join('a', 'b1', '..', 'b2') === 'a/b1/../b2'
	path.resolve('a', 'b1', '..', 'b2') === 'a/b2'
	```

	一目了然，path.join就是用/将参数连接起来，而path.resolve返回的是执行操作最后的目录

* 今天在用express做图片上传的时候，node server提示我413，express默认允许上传的大小为1MB，因此需要手动修改，通过增加`app.use(bodyParser.json({limit: '50mb'}));`就可以放开限制

* 记录一下npm发包到npmjs的步骤

	首先，需要在www.npmjs.org注册一个账号，这个账号会被添加到npm本地的配置中，用来发布module用。
	
	```
	$ npm adduser
	Username: your name
	Password: your password
	Email: yourmail@gmail.com
	```	
	
	如果出现错误的话，那么有可能是你的npm版本过低，可以升级npm试试
	
	成功之后，npm会把认证信息存储在~/.npmrc中，并且可以通过以下命令查看npm当前使用的用户：
	
	```
	$ npm whoami
	```
	
	以上完成之后，我们终于可以发布自己的module了：
	
	```
	$ cd repos
	$ npm publish --tag 0.1.0
	```
	
	这样，我们就成功发了一个npm包到npmjs上了，下面讲一下npm社区版本号规则
	
	版本格式：主版号.次版号.修订号，版号递增规则如下：
	
	* 主版号：当你做了不相容的 API 修改
	* 次版号：当你做了向下相容的功能性新增
	* 修订号：当你做了向下相容的问题修正
	* 先行版号及版本编译资讯可以加到「主版号.次版号.修订号」的后面，作为延伸
	
* nodejs调用RPC服务

    RPC（Remote Procedure Call Protocol）——远程过程调用协议，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。RPC协议假定某些传输协议的存在，如TCP或UDP，为通信程序之间携带信息数据。在OSI网络通信模型中，RPC跨越了传输层和应用层。RPC使得开发包括网络分布式多程序在内的应用程序更加容易。RPC采用客户机、服务器模式。请求程序就是一个客户机，而服务提供程序就是一个服务器。首先，客户机调用进程发送一个有进程参数的调用信息到服务进程，然后等待应答信息。在服务器端，进程保持睡眠状态直到调用信息的到达为止。当一个调用信息到达，服务器获得进程参数，计算结果，发送答复信息，然后等待下一个调用信息，最后，客户端调用进程接收答复信息，获得进程结果，然后调用执行继续进行。有多种RPC模式和执行。最初由Sun公司提出。IETF ONC宪章重新修订了Sun版本，使得ONC RPC协议成为IETF标准协议。现在使用最普遍的模式和执行是开放式软件基础的分布式计算环境（DCE）
    
	1. 通过Thrift生成node语言代码
		
		brew install thrift
		cd 目录
		thrift -r --gen js:node name.thrift
	
	2. 配置thrift.json

		如果是部署在开发机上的rpc服务，知道Host和Port就行了，如果是线上环境，则需要通过psm来访问
	
	3. nodejs调用rpc服务

		```js
		
		import thrift from "thrift"
		import XXXService from "./name-nodejs/XXXService"
		import ttypes from "./name-nodejs/XXXTypes"
		 
		const ThriftConfig = require("../../config/thrift.json");
		 
		export default class RPC{
 
    	constructor(){
        	this.connect();
    	}
 
	    	connect(){
		        this.connection = thrift.createConnection(ThriftConfig[0].Host, ThriftConfig[0].Port);
		        this.client = thrift.createClient(XXXService, this.connection);
		        this.ttypes = ttypes;
		 
		        this.connection.on("error",function(e) {
		            logger.error("RPC Connect Error..")
		        });
		 
		        this.connection.on("connect", function(e){
		            logger.info("RPC Connect Success..")
		        });
		    }
		 
		    //构建参数
		    setStatRequest(objStat){
		 
		        if(Object.prototype.toString.call(objStat) !== '[object Object]'){
		            logger.warn("Params objStat error");
		        }
		 
		        return new this.ttypes.StatRequest(objStat);
		    }
		 
		    //RPC通信
		    getObjectStatMetric(ObjectStruct){
		        this.connect();
		        let res = this.client.get_object_stat_metric(ObjectStruct);
		        //this.close();
		        return res;
		 
		    }
		 
		    close(){
		        this.connection.end();
		        this.connection.on("close", function(e){
		            logger.info("RPC Connect close..")
		        });
		    }
	 
		}

		```

* node实用模块(长期更新)

  * svg-captcha 一个图片，文字验证码的生成包，可以用于登录界面的使用
    
	    ```js
	  	 <img src="/api/getCaptcha" alt="captcha" />
	
		 router.get('/api/getCaptcha', function(req, res, next) {
		   var captcha = svgCaptcha.create({  
		      // 翻转颜色  
		      inverse: false,  
		      // 字体大小  
		      fontSize: 36,  
		      // 噪声线条数  
		      noise: 2,  
		      // 宽度  
		      width: 80,  
		      // 高度  
		      height: 30,  
		    });  
		    // 保存到session,忽略大小写  
		    req.session = captcha.text.toLowerCase(); 
		    console.log(req.session); //0xtg 生成的验证码
		    //保存到cookie 方便前端调用验证
		    res.cookie('captcha', req.session); 
		    res.setHeader('Content-Type', 'image/svg+xml');
		    res.write(String(captcha.data));
		    res.end();
		 });
	    ```