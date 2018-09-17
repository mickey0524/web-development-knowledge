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
