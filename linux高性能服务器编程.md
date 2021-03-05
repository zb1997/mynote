





3.6 TCP交互数据

3.7 TCP成块数据

3.8 带外数据(Out Of Band)

3.9 TCP超时重传

1. tcp重传策略：TCP一共执行5次重传。每次重传的时间都增加一倍，在重传失败的情况下，底层的IP和ARP开始接管连接，Linux有两个重要的内核参数与TCP超时重传相关： /proc/sys/net/ipv4/tcp_retries1和/proc/sys/net/ipv4/tcp_retries2。 前者指定在底层IP接管之前TCP最少执行的重传次数， 默认值是3。 后者指定连接放弃前TCP最多可以执行的重传次数， 默认值是15（一般对应13～30 min）   
2. 虽然超时会导致重传，但是重传可以发生在超时之前，即快速重传

3.10 拥塞控制   慢开始 拥塞避免 快重传 快恢复

​	https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%20-%20%E4%BC%A0%E8%BE%93%E5%B1%82.md这篇文章中有比较形象的说明

#### 第四章：TCP／IP通信案例

### 第二篇：深入解析高性能服务器编程

#### 第五章：linux网络编程基础API

1. socket地址api：
   1. 主机序(小端)和网络序(大端)
   2. 通用sockaddr   结构体sockaddr
   3. 专用sockaddr   结构体sockaddr_un sockaddr_in sockaddr_in6
   4. IP地址转换函数
2. 创建socket、命名、监听
3. 连接相关：接受、发起、关闭
4. 数据读写
5. 带外标记
6. 地址信息函数
7. scoket选项
8. 网络信息api