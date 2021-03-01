##### 事件匹配流程

1. 程序启动app注册的时候会将事件处理函数一起注入ryuapp中
2. 控制器的_recv_loop收到事件之后开始解包并且匹配事件之后，调用send_ev_to_observer，压入ryuapp的事件队列中
3. event_loop函数读队列中的事件，分配给对应的函数进行处理