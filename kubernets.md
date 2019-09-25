# kubernetes

k8s的架构是一个经典的分布式一致性架构  
用户把期望的操作(比如发布多少个pod)通过api server写入etcd，先落地  
然后scheduler/controller manager配合进行调度协调，把用户期望的操作以最终一致方式实现出来  
etcd即是一个db，也是一个bus，具有通知功能  
把要做什么(expectation)先落地(etcd)，然后协调器/控制器/kubelet去干活实现这个expectation，不管成功失败超时，都把这个结果报告回来(etcd)，然后继续协调到最终一致，或者干不下去人工干预