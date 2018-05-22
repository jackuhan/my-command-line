# my-command-line常用命令行 

------

### 查看进程 
```shell
ps -ef |grep jetty  
ps aux |grep java
```

### 批量杀死java进程
```shell
pkill java
```
### 杀死进程
```shell
kill -9 42947
```

### 查看性能问题
日志
堆栈
jmap
jheap
jstack
```shell
grep "java.lang.Thread.State" 20180322.log | awk -F' ' '{print $2}' | sort | uniq -c
```
```shell
jstack 15562 > /tmp/20180322.log
```
```shell
grep "java.lang.Thread.State" 20180317_1.txt | wc -l
```
wc -l用来统计数量，上面用来统计线程数量。
```shell
grep -A 30 -B 1
du -sh *
df -h //硬盘信息
free -m
jmap -heap 15562 > /tmp/2.log
jmap  histro:current pid   查看现在内存对象实例

>  //清空文件在写入
>>  //不清空，写入
```

### 查看性能问题2
1，使用命令top -p <pid> ，显示你的java进程的内存情况，pid是你的java进程号，比如123
2，按H，获取每个线程的内存情况
3，找到内存和cpu占用最高的线程pid，比如15248
4，执行 printf 0x%x 15248 得到 0x3b90 ,此为线程id的十六进制
5，执行 jstack 123|grep -A 10 3b90，得到线程堆栈信息中3b90这个线程所在行的后面10行
6，查看对应的堆栈信息找出可能存在问题的代码

JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解
https://my.oschina.net/feichexia/blog/196575

### 查询端口号
```shell
lsof -i tcp:port 
lsof -i:8080
```
```shell
netstat -plten |grep java
ssh root@gamebox-tomcat-online011-jylt.qiyi.virtual -i /路径/id_rsa
```
### #Linux – Which application is using port 8080
http://www.mkyong.com/linux/linux-which-application-is-using-port-8080/

### 查看命令历史使用记录并通过less分页显示
```shell
history | less
```
```shell
which       //查看可执行文件的位置。
whereis     //查看文件的位置。 
locate      //配合数据库查看文件位置。
find        //实际搜寻硬盘查询文件名称。
```


### 查看当前机器ip
```shell
ip address show
```

### 查看日志
tail
more
head
grep
vi

### 查看代码log日志
```shell
find . -name "*.log"
find . -type f -name "*.log"
find . -name “*tomcat*” 
find . -name "server.xml"
```
```shell
tail -f |grep exception ./data/logs/xxx.log 
```

### 根据关键字查询日志
```shell
tail -f  ./data/logs/xxx.log   | grep 7370 -i -A 20
tail -f  ./data/logs/xxx.log  | grep exception -i -A 20
tail -f  ./data/logs/xxx.log |grep ???
tail -f  xxx.log  | grep exception -i -A 20
```

### 查看实时日志
```shell
tail -f  ./data/logs/xxx.log | grep exception -i -A 20
tail -f  ./data/logs/xxx.log | grep AbstractGCController -i -A 20 --color
tail -f  ./data/logs/xxx.log | grep AbstractGCController -i -C 20 --color
```

### -d表示高亮不同的地方，-n表示多少秒刷新一次。
```shell
watch -d -n 5 cat xxx.log
tail -n 1000        //显示最后1000行
tail -n +1000       //从1000行开始显示，显示1000行以后的
head -n 1000        //显示前面1000行

grep -A 5       //可以显示匹配内容以及后面的5行内容
grep -B 5       //可以显示匹配内容以及前面的5行内容
grep -C 5       //可以显示匹配内容以及前后面的5行内容
grep -i         //不区分大小写
```

### 自动给grep加颜色
```shell
1. vim ~/.bashrc   
2. alias grep='grep --color'  
3. source ~/.bashrc  
```
```shell
grep -i ’/gamecenter/top/rankNew/’xxx.log 
```


### tomcat日志

查找日志所在目录
```shell
1、ps -ef |grep tomcat  
```
```shell
root      5667     1  0 Apr16 ?        00:32:35 /usr/local/jdk8/jre/bin/java 

-Djava.util.logging.config.file=/opt/soft/apache-tomcat-9.0.0.M10/conf/logging.properties 

-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager 

-Djdk.tls.ephemeralDHKeySize=2048 -classpath 
/opt/soft/apache-tomcat-9.0.0.M10/bin/bootstrap.jar:/opt/soft/apache

-tomcat-9.0.0.M10/bin/tomcat-juli.jar 

-Dcatalina.base=/opt/soft/apache-tomcat-9.0.0.M10 

-Dcatalina.home=/opt/soft/apache-tomcat-9.0.0.M10 

-Djava.io.tmpdir=/opt/soft/apache-tomcat-9.0.0.M10/temp org.apache.catalina.startup.Boots
```
```shell
cd /opt/soft/apache-tomcat-9.0.0.M10/
```
```shell
2、ps -ef |grep tomcat 
root      7534  7483  0 15:10 pts/0    00:00:00 grep --color=auto tomcat
root     16637     1  1 11:47 ?        00:02:38 /opt/java/jdk1.8.0_131/jre/bin/java -Djava.util.logging.config.file=/opt/tomcat/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Djava.endorsed.dirs=/opt/tomcat/endorsed -classpath /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar -Dcatalina.base=/opt/tomcat -Dcatalina.home=/opt/tomcat -Djava.io.tmpdir=/opt/tomcat/temp org.apache.catalina.startup.Bootstrap start
```
```shell
cd /opt/tomcat
```
```shell
/usr/local/tomcat/conf/server.xml
/usr/local/tomcat/bin/catalina.sh
```
```shell
grep -i 'JAVA_OPTS' catalina.sh
```

其中JAVA_OPTS 设置了jvm参数

### find命令
```shell
find . -name "*.log"
find . -type f -name "*.log"
find . -name “*tomcat*” 
find . -name "server.xml"
```

### vi
```shell
vi catalina.out
```
win + f 屏幕『向下』移动一页，相当于 [Page Down]按键
win+ b 屏幕『向上』移动一页，相当于 [Page Up] 按键
:$ 跳到文件最后一行
:0或:1 跳到文件第一行
　　或 另外一组命令：
　　gg 跳到文件第一行
Shift + g 跳到文件最后一行


### 查找指定文件的关键词
```shell
grep -i 'ERROR' catalina.out
grep -i ’17:20:’ catalina.out
```


##nginx日志
```shell
/data/nginx/logs/gamecenter-http.log
/opt/logs/nginx/access.log
/opt/nginx
```

### zookeeper
```shell
zkServer start zoo.cfg
zkServer status
zkServer status  zoo.cfg
zkServer stop zoo.cfg
telnet 127.0.0.1 2181
```