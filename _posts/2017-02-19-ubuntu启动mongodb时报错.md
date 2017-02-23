---
layout:     post
title:      "解决启动mongod 时，出现addr already in use错误"
subtitle:   "Ubuntu settings allow root users to log in"
date:       2017-2-19 12:00:00
author:     "Strom"
header-img: "img/home-bg-art.jpg"
---


启动mongod   root@VM-118-87-ubuntu:/usr/bin# ./mongod

<b>错误提示：</b>

```
Sat Aug 17 09:02:02 [initandlisten] ERROR: listen(): bind() failed errno:98 Address already in use for socket: 0.0.0.0:27017
Sat Aug 17 09:02:02 [initandlisten] ERROR:   addr already in use


```

原因是启动mongod时端口被占用。
<b> 解决办法：</b>

使用命令 root@VM-118-87-ubuntu:/usr/bin# netstat -anp|more
可以看到客户端还保持着与服务器的连接

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       
PID/Program name
tcp        0      0 127.0.0.1:28017         0.0.0.0:*               LISTEN      
953/mongod      
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      
1546/dnsmasq    
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      
653/cupsd       
tcp        0      0 127.0.0.1:27017         0.0.0.0:*               LISTEN      
953/mongod      
tcp        1      0 192.168.5.124:56874     91.189.89.144:80        CLOSE_WAIT  
1913/ubuntu-geoip-p
tcp        0      1 192.168.5.124:45890     220.181.111.24:80       FIN_WAIT1   
-               
tcp        0      0 192.168.5.124:44867     219.148.35.218:80       ESTABLISHED
2219/firefox    



```
杀死953进程root@VM-118-87-ubuntu:/usr/bin#

```

kill -9 953
```



再次察看服务器链接状态：

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       
PID/Program name
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      
1546/dnsmasq    
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      
653/cupsd       
tcp        0      0 192.168.5.124:60805     61.172.207.130:80       TIME_WAIT   
-               
tcp        1      0 192.168.5.124:56874     91.189.89.144:80        CLOSE_WAIT  
1913/ubuntu-geoip-p
tcp        0      0 192.168.5.124:54370     117.79.157.237:80       TIME_WAIT   
-               



```
OK已经成功杀掉进程。

root@VM-118-87-ubuntu:/usr/bin#

```
./mongod

```


```
Sat Aug 17 09:32:25 [initandlisten] waiting for connections on port 27017
Sat Aug 17 09:32:25 [websvr] admin web console waiting for connections on port 28017

```
问题解决。
