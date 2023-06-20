# 简述Keepalived及其工作原理？

Keepalived 是一个基于VRRP协议来实现的LVS服务高可用方案，可以解决静态路由出现的单点故障问题。

在一个虚拟路由器中，只有作为MASTER的VRRP路由器会一直发送VRRP通告信息,BACKUP不会抢占MASTER，除非它的优先级更高。当MASTER不可用时(BACKUP收不到通告信息)
多台BACKUP中优先级最高的这台会被抢占为MASTER。这种抢占是非常快速的(<1s)，以保证服务的连续性
由于安全性考虑，VRRP包使用了加密协议进行加密。BACKUP不会发送通告信息，只会接收通告信息

# 简述Keepalived体系主要模块及其作用？

keepalived体系架构中主要有三个模块，分别是core、check和vrrp。

core模块为keepalived的核心，负责主进程的启动、维护及全局配置文件的加载和解析。

vrrp模块是来实现VRRP协议的。

check负责健康检查，常见的方式有端口检查及URL检查。

# 简述Keepalived如何通过健康检查来保证高可用？

Keepalived工作在TCP/IP模型的第三、四和五层，即网络层、传输层和应用层。

网络层，Keepalived采用ICMP协议向服务器集群中的每个节点发送一个ICMP的数据包，如果某个节点没有返回响应数据包，则认为此节点发生了故障，Keepalived将报告次节点失效，并从服务器集群中剔除故障节点。利用 Nginx+Keepalived 实现高可用技术

传输层，Keepalived利用TCP的端口连接和扫描技术来判断集群节点是否正常。如常见的web服务默认端口80，ssh默认端口22等。Keepalived一旦在传输层探测到相应端口没用响应数据返回，则认为此端口发生异常，从而将此端口对应的节点从服务器集群中剔除。

应用层，可以运行FTP、telnet、smtp、dns等各种不同类型的高层协议，Keepalived的运行方式也更加全面化和复杂化，用户可以通过自定义Keepalived的工作方式，来设定监测各种程序或服务是否正常，若监测结果与设定的正常结果不一致，将此服务对应的节点从服务器集群中剔除。

Keepalived通过完整的健康检查机制，保证集群中的所有节点均有效从而实现高可用。



# 出现keepalived脑裂，是什么原因？

由于某些原因，导致两台keepalived高可用服务器在指定时间内，无法检测到对方存活心跳信息，从而导致互相抢占对方的资源和服务所有权，然而此时两台高可用服务器有都还存活。可能出现的原因:
1、服务器网线松动等网络故障;
2、服务器硬件故障发生损坏现象而崩溃；
3、主备都开启了firewalld 防火墙。