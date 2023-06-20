# 你知道的web服务有哪些？

apache

nginx

tomcat

weblogic

IIS

lighttpd

#  nginx实现负载均衡的分发策略

Nginx 的 upstream目前支持的分配算法：	

1)、轮询 ——1：1 轮流处理请求（默认）

2)、权重 ——you can you up

 通过配置权重，指定轮询几率，权重和访问比率成正比，用于应用服务器性能不均的情况。

3)、ip_哈希算法

 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个应用服务器，可以解决session共享

的问题。

# nginx做负载均衡用到哪些模块

upstream 定义负载节点池。

location 模块 进行URL匹配。

proxy模块 发送请求给upstream定义的节点池



# 请解释一下什么是Nginx ?

# 请列举Nginx的一些特性。

# 请列举Nginx和Apache 之间的不同点。

# 请解释Nginx如何处理HTTP请求。

# 在Nginx中，如何使用未定义的服务器名称来阻止处理请求?

# 使用“反向代理服务器”的优点是什么?

# 请列举Nginx服务器的最佳用途。

# 请解释Nginx服务器上的Master和Worker进程分别是什么?

# 请解释你如何通过不同于80的端口开启Nginx?

# 请解释是否有可能将Nginx的错误替换为502错误、503

# 在Nginx中，解释如何在URL中保留双斜线

# 请解释ngx_http_upstream_module的作用是什么

# 请解释什么是C10K问题

# 请陈述stub_status和sub_filter指令的作用是什么

# nginx如何实现反向代理

```
proxy_pass IP
```



