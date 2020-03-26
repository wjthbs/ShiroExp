# ShiroExploit GUI Version
支持对Shiro550（硬编码秘钥）和Shiro721（Padding Oracle）的一键化检测

## 使用说明 
![pic1](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic1.png)
+ 按要求输入要检测的目标URL和选择漏洞类型
+ Shiro550无需提供rememberMe Cookie，Shiro721需要提供一个有效的rememberMe Cookie

![pic2](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic2.png)
+ 如果选择```和CEYE配合使用```，则无需进行任何配置，程序中已经内置了CEYE域名和对应的Token，即使多人同时使用，也不会存在干扰问题。
+ 如果选择```和自实现的OOB Service```配合使用，则需要通过命令```java -cp ShiroExploit.jar com.shiroexploit.server.BasicHTTPServer [HttpSerivce Port] [JRMPListener Port]```开启OOB Service，并按照要求填入相应IP和端口
+ 如果开启OOB Service时未指定端口号，则HTTPService默认监听8080端口，JRMPListener默认监听8088端口

![pic3](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic3.png)
![pic4](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic4.png)
+ 程序在判断目标应用是否存在漏洞时，命令输入框无法进行输入。当程序检测出目标应用存在漏洞时，命令输入框可以进行输入并执行命令。

## 备注
在使用漏洞检测主程序或者使用OOBService时，均需要ysoserial.jar的支持，将ysoserial.jar和ShiroExploit.jar放置在同一目录即可。
