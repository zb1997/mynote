## 架构图

![img](file:///C:/Users/Administrator/Documents/My Knowledge/temp/860ea510-0f43-4c64-aff1-984fef1b48e9/128/index_files/Image.png)

> ovs-vswitchd： 主要模块，实现switch的daemon，包括一个支持流交换的Linux内核模块；
> ovsdb-server： 轻量级数据库服务器，提供ovs-vswitchd获取配置信息，例如vlan、port等信息；
> ovs-brcompatd： 让ovs-vswitch替换linux bridge，包括获取bridge ioctls的Linux内核模块；
> ovs-dpctl：用来配置switch内核模块；
> ovs-vsctl： 查询和更新ovs-vswitchd的配置；
> ovs-appctl： 发送命令消息，运行相关daemon；
> ovs-ofctl： 查询和控制OpenFlow交换机和控制器；
> ovs-openflowd：一个简单的OpenFlow交换机；
> ovs-controller：一个简单的OpenFlow控制器；
> ovs-pki：OpenFlow交换机创建和管理公钥框架；
> ovs-tcpundump：tcpdump的补丁，解析OpenFlow的消息；
> ovs-bugtool：管理openvswitch的bug信息。

![img](file:///C:/Users/Administrator/Documents/My Knowledge/temp/860ea510-0f43-4c64-aff1-984fef1b48e9/128/index_files/e510e3c78f727c37cef7c58ff6213d51.png)

主要的动作通过ovs-xxctl -h 进行搜索，实在不行再进行百度