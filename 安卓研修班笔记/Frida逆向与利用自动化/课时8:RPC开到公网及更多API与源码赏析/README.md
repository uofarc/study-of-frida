# RPC开到公网以及更多API与源码赏析
# 更多API
1. frida(rpc)开到公网集群+(或flask)
2. child-gating,上传到PC打印
3. ZenTrace
4. FRIDA-DEXDump
5. objection

# 0x01 frida(rpc)开到公网集群+（或flask）
这里太骚了，本来以为手机是内网，如果不在一个路由器下，rpc就难调用了，没想到啊，这波是直接通过公网的一台服务器映射到内网的ip和端口上，那么只要我们rpc到公网的ip和端口，那么直接转发到内网机器进行操作，直接起飞，多台手机甚至可以同时操作，细思极恐，问题我服务器首先没有，淦，其次一些计网知识直接缺失了，有点难过，现在去补补看看，实在不行，服务器还是得整上！  
___  
阿里云买了台vps，感觉还是蛮香的，立马安装上了nps，然后在阿里云的安全组里，把8080端口打开了，之后ip加8080端口就可以访问后台的web端了.  
安装参考链接:  
https://www.cnblogs.com/jimaojin/p/12381656.html  
https://ehang-io.github.io/nps/#/nps_use  
___
下一步开始配置我的客户端
1. 首先安装客户端的nps，下载好tar文件，然后解密
2. 之后去服务端打开nps后台，新建一个客户端，名字备注一下，其他都不用填
3. 然后新建一个tcp隧道，服务端端口随意，客户端端口填frida-server运行的非标准端口
4. 然后点击nps后台的客户端，发现我们之前创建的客户端存在了，然后点击左边的加号，出现一个客户端命令，类似./npc -server=xxx.xxx.xxx:8024 -vkey=XXX -type=tcp
5. 把这行命令复制下来，粘贴到手机上对应之前解压那个客户端nps的目录下，直接运行
6. 如果一切顺利，发现web端nps的客户端会显示已经连接，说明一切正常
7. 最后别忘记启动frida-server在你之前新建隧道时填写的端口号
8. 在python代码中的remote_devices方法，里面就不用和之前一样填写手机的ip和端口了,
可以直接填写服务端的ip和tcp隧道上我们填写的服务端端口，因为已经内网穿透了

我这里又报错了，发现remote不到frida-server，有点迷，还得问问
# 0x02 ZenTrace
哭了，又报错说python版本和frida版本对不上。自闭
ZenTrace的源码分析，这里我没办法操作，就只能看了
# 0x03 Frida-DEXDump源码分析
从内存中去暴力搜索，根据dex头去搜索