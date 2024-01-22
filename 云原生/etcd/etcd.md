[TOC]



# 介绍

> https://etcd.io/

etcd是一个强一致性、分布式的键值存储，提供了一个稳定的方式来存储数据，供一个分布式系统或一组机器访问。它优雅地处理了网络分区中的leader选举，并且能够容忍机器宕机，即使是主（leader）结点。

> **etcd** is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. It gracefully handles leader elections during network partitions and can tolerate machine failure, even in the leader node.

etcd可以容忍集群中部分节点故障，只需存活一半以上节点即可对外提供服务，主要用于元数据存储、服务发现、分布式选举等场景。基于 etcd 提供的 Watch 机制，还可以便捷地实现发布订阅等功能。

分布式系统中的数据分为控制数据和应用数据。etcd的使用场景默认处理的数据都是控制数据，对于应用数据，只推荐数据量很小，但是更新访问频繁的情况。应用场景有如下几类：

- 服务发现（Service Discovery）
- 消息发布与订阅
- 负载均衡
- 分布式通知与协调
- 分布式锁、分布式队列
- 集群监控与Leader竞选

目前，cloudfoundry使用etcd作为hm9000的应用状态信息存储，kubernetes用etcd来存储docker集群的配置信息等。

# 工作原理

> https://zhuanlan.zhihu.com/p/405811320

ETCD使用Raft协议来维护集群内各个节点状态的一致性。简单说，ETCD集群是一个分布式系统，由多个节点相互通信构成整体对外服务，每个节点都存储了完整的数据，并且通过Raft协议保证每个节点维护的数据是一致的。

![img](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311262144376.png)

如图所示，每个ETCD节点都维护了一个状态机，并且，任意时刻至多存在一个有效的主节点。主节点处理所有来自客户端写操作，通过Raft协议保证写操作对状态机的改动会可靠的同步到其他节点。

ETCD工作原理核心部分在于Raft协议。本节接下来将简要介绍Raft协议，具体细节请参考其[论文]。
 Raft协议正如论文所述，确实方便理解。主要分为三个部分：选主，日志复制，安全性。