# k8s知识点

### 一、两种节点

![image-20231211103057492](C:\Users\Thinkpad\AppData\Roaming\Typora\typora-user-images\image-20231211103057492.png)

1） master节点

1.1) api-server

​		其他客户端  通过api-server  操作 组件资源的增删改查，api-server对客户端做认证授权等安全措施，api-server将

1.2) controller-manager

​		watch api-server，查询到 对资源对象的 增删改查的操作后，调用相应的 controller做相应的解析，并将解析完后的结果信息返回给api-server，然后由api-server将整合完的信息更新到 etcd中。

1.3) scheduler

​		scheduler负责集群资源调度，其作用是将待调度的pod通过一系列复杂的调度算法计算出最合适的node节点，然后将pod绑定到目标节点上。shceduler会根据pod的信息，全部节点信息列表，过滤掉不符合要求的节点，过滤出一批候选节点，然后给候选节点打分，选分最高的就是最佳节点，scheduler就会把目标pod安置到该节点。

1.4) etcd

1.5) kubelet

​		每个node节点上都有一个kubelet服务进程，kubelet作为连接master和各node之间的桥梁，负责维护pod和容器的生命周期，当监听到master下发到本节点的任务时，比如创建、更新、终止pod等任务，kubelet 即通过控制docker来创建、更新、销毁容器；
​		每个kubelet进程都会在api-server上注册本节点自身的信息，用于定期向master汇报本节点资源的使用情况。

1.6) kube-proxy



以创建一个pod为例：

![create_pod_flow](C:\Users\Thinkpad\Desktop\jobInterviews\互联互通知识点\create_pod_flow.png)

1）kubectl 命令 将新建pod的yaml传给api-server

2）api-server将yaml信息保存到 ectd中

3）controller-manager watch到 api-server 的新建一个pod的信息，RS -controller就开始解析 该 pod依赖的pod信息，将整理完的信息给到 api-server，然后api-server再更新到 etcd中。

4）scheduler 通过 watch 到 api-server的 这个 pod可以被调度了，分配一个node给这个pod，再将node的地址和这个pod的绑定信息传给api-server,然后再更新到 etcd中。然后将pod交给kubelet。

5）kubelet在收到pod后，调用 CRI创建 容器，调用 CSI 进行存储卷的挂载。

6）等待 网络、存储、容器的创建完成，再等业务进程完成初始化，pod完成创建。