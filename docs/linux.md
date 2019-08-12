* lsof -i:port

	在开发过程中，经常出现端口被占用的情况，使用这个指令可以检测端口是否被占用

* 实用vim指令

	[实用vim指令](https://linux.cn/article-8144-1.html)

* curl的基本用法

    * curl执行get请求，`curl url`就可以
    * curl执行post请求 `curl -d 'args(key1=value1&key2=value2)' url`
    * 从chrome中快速获取curl命令的方法 `控制台->Network->XHR->右键Copy->copy as cURL`

* linux重定向指令`>`和`>>`的区别

    `>`是覆盖写，`>>`是追加写

* linux中打log经常可以看到`2>&1`

    2代表stderr，1代表stdout，`2>&1`代表将错误信息同样重定向到指定输出文件中

* ln [-sf] source\_file target\_file linux 链接

	默认是 hard link，加上 -s 指代为符号链接

* linux 指令与文件搜索

	* which 指令搜索

		```
		which python -> /usr/local/bin/python
		```
		
	* whereis 文件搜索，速度比较快，因为只搜索几个特定的目录

		```
		whereis python -> /usr/bin/python
		```
		
	* find [basedir] [option] 文件搜索，可以使用文件的属性和权限进行搜索

		```
		find . -name 'pyth*'
		```
		
* 数据流重定向

	重定向指的是使用文件代替标准输入、标准输出和标准错误输出
	
	* 标准输入 stdin < 或 <<
	* 标准输出 stdout > 或 >>
	* 标准错误输出 stderr 2> 或 2>>
	
	其中，有一个箭头的表示以覆盖的方式重定向，而有两个箭头的表示以追加的方式重定向
	
	如果需要将标准输出以及标准错误输出同时重定向到一个文件，需要将某个输出转换为另一个输出，例如 2>&1 表示将标准错误输出转换为标准输出
	
	```
	$ find /home -name .bashrc > list 2>&1
	```

* ps 查看进程

	* ps -l 查看自己的进程
	* ps aux 查看系统所有进程
	* ps aux | grep 条件 查看某一条件进程

* top 实时显示进程信息

* netstat 查看占用端口的进程

	```
	netstat -anp | grep port 查看某一端口进程
	```

* Linux 中的 select, poll 以及 epoll

	* [Linux IO模式及 select、poll、epoll详解](https://segmentfault.com/a/1190000003063859)
    * [Linux IO](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/Socket.md)

* grep 处理的是哪个 fd？

    处理的是 `std_out + std_err`

* Socket 的 accept 方法是用来干什么的，在三次握手中属于哪一次？

    accept 从 client 已经连接上的链表中按照 FIFO 的方式取出一个连接，我觉得不属于三次握手，应该在三次握手之后，就算在三次握手之前调用也是阻塞的
