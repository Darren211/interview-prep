# Kubernetes 高频面试题

## Pod 如何对外提供访问？

可用 Service 为一组 Pod 提供稳定访问入口；集群外访问可结合 NodePort、LoadBalancer 或 Ingress/Gateway。`hostPort` 直接占用节点端口，调度和端口冲突管理更受限制，通常不是首选通用暴露方式。

## iptables 和 IPVS 有什么区别？

两者都可用于 kube-proxy 的 Service 转发实现。iptables 依赖规则匹配，IPVS 使用内核虚拟服务器机制，面向大量服务时通常有更好的扩展特性和多种调度算法。实际性能要结合内核版本、规则规模和流量验证，不能只用 O(1)/O(n) 概括全部成本。

## Pod 创建流程是什么？

请求进入 API Server 后经过认证、授权、准入控制并写入 etcd。Scheduler 观察未调度 Pod，过滤并打分候选节点后完成绑定。目标节点 kubelet 观察到 Pod，调用容器运行时准备 sandbox、容器和网络，并持续汇报状态。控制器可能先创建或调整 Pod 对象。

## ReplicaSet 和 Deployment 有什么区别？

ReplicaSet 确保指定数量的 Pod 副本存在。Deployment 管理 ReplicaSet，并提供声明式更新、滚动发布和回滚等能力。

## etcd 使用 Raft 如何选主和复制日志？

Follower 超时未收到心跳后可发起选举；候选者获得多数票后成为 Leader。Leader 接收日志并复制给集群，多数节点确认后条目可提交。选票和日志新旧判断受任期与日志索引约束。

## Label 和 Annotation 有什么区别？

Label 用于选择器筛选和组织对象；Annotation 用于附加说明性元数据，通常不参与选择。两者都属于对象元数据，并有各自的键值格式与大小限制。

## 什么是静态 Pod？

静态 Pod 由指定节点上的 kubelet 根据本地清单管理，通常用于启动控制平面组件。API Server 可显示其镜像对象，但不能像普通 Pod 一样直接管理其生命周期。

## 常见调度约束有哪些？

NodeSelector、NodeAffinity、Pod 亲和/反亲和、污点与容忍、资源请求，以及拓扑分布约束等。Scheduler 通常经过过滤和打分阶段选择节点。

## Informer 和工作队列的作用是什么？

Informer 通过 List/Watch 维护本地缓存并分发对象变化事件；控制器通常将 key 放入工作队列，异步协调实际状态与期望状态。限速队列可支持重试和退避。处理逻辑应幂等，并正确处理缓存最终一致性。

## RBAC 的基本对象有哪些？

Role/ClusterRole 定义权限规则，RoleBinding/ClusterRoleBinding 将规则绑定到 User、Group 或 ServiceAccount。Role 作用于命名空间，ClusterRole 可定义集群范围权限或复用规则。
