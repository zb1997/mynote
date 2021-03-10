## 架构图

![image-20210303190912191](https://github.com/zb1997/mynote/blob/main/tupian/image-20210303190912191.png)

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

1. OVS有内核模块datapath和用户空间的vsiwtchd和ovsdb组成

   1. datapath负责数据转发，从网卡中读取数据，并快速匹配流表中的流表项实现快速转发，如果没有匹配到则上交给vswitch处理。
   2. vswitchd时ovs的核心模块，负责与openflow控制器和其他第三方软件进行通信
   3. ovsdb用于存储ovs具体配置的数据库，
   4. 数据包匹配：
      1. 如果datapath直接匹配到内核中缓存的流表项，直接进入转发通到，无需upcall到上层
      2. 如果在内核没有匹配到流表项，那就upcall到上层，在经过vswitchd处理之后通过netlink将数据交给datapath转发，并更新流表。vswtichd的处理流程为先对用户空间流表进行匹配，成功则交给datapath进行转发，并将对应的流表项写入内核缓存，如果失败，则会根据openflow版本规范处理，比如上报给控制器或者丢弃，如果控制器处理成功则下发flow-mod报文，ovs将对应的flow-mod报文中的流表向写入用户态的流表空间，用于后续匹配
      3. 内核空间的缓存空间很小，存放的都是最新命中的最高命中率的流表项

2. 流表匹配顺序：

   1. 在2.*(某个版本)之后的版本，将单纯的精确匹配改换成了精确匹配+模糊匹配的方式，
   2. 两条原则：
      1. 优先级priority，优先级分布在0-65535，数值越高，优先级越高
      2. 相同优先级，不同的区域匹配程度，按添加的时间先后顺序进行匹配
   
3. 修改流表的相关指令：

   ```shell
   sudo ovs-ofctl -O OpenFlow add-flow br0 "flows"
   sudo ovs-ofctl -O OpenFlow del-flows br0 "flows"
   sudo ovs-ofctl -O OpenFlow add-group br0 group_id=1,type=select,bucket=output:1
   
   ```

   

4. ovs配置gre隧道

   ```shell
   sudo ovs-ctl start
   sudo ovs-vsctl add br0
   sudo ovs-vsctl set bridge br0 fail-mod=secure other-config:datapath-id=01 other-config:hwaddr=00:00:00:00:00:01
   sudo ip a a 10.0.0.1/8 dev br0
   sudo ip l s br0 up
   sudo ovs-vsctl add-port br0 gre1 --set interface gre1 type gre option:remote_ip=193.138.56.1
   sudo ovs-vsctl set-controller br0 tcp:ip:port
   ```

   

![image-20210303190928839](https://github.com/zb1997/mynote/blob/main/tupian/image-20210303190928839.png)

主要的动作通过ovs-xxctl -h 进行搜索，实在不行再进行百度