+++
date = "2017-03-06T12:50:27+08:00"
title = "ambari端口修改"
categories = ["运维"]
tags = ["大数据"]

+++

Ambari 使用 `8080` 端口提供服务，这个端口很多情况下会被 tomcat 等其他应用所占用。修改的方法如下：


修改配置文件 `/etc/ambari-server/conf/ambari.properties`

```
client.api.port=<port_number>
```

默认情况下配置文件中没有这个选项，添加上就可以。


