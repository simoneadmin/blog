1、首先排查微服务，没有发现JVM异常退出；（查看启动目录是否有hs_err_pid***.log）

2、查看log日志，如图：





发现服务于下午14:11被系统kill了。

3、查找服务器上历史操作记录，如下：





发现有一个salt重启的一个动作，然后查看/var/log/message日志，发现如图：





salt重启时间和服务停止时间一致，则初步判断是salt引起的异常；

造成异常原因分析：

启动服务的命令是 ：runas = tomcat，停止salt客户端之后，服务会强制被kill。

改善方案：

将runas = tomcat 改成 su -u tomcat -c 方式启动即可。

具体为什么停止salt会导致后台服务停止，还需要进一步研究。



