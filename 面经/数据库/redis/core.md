# Redis 高频面试题

## Redis 常见数据结构有哪些？

String、List、Set、Sorted Set、Hash 是常见抽象数据类型。具体底层编码会随 Redis 版本和数据规模变化，例如小对象可能使用紧凑编码，不能把抽象结构与单一底层实现一一等同。Bloom Filter 通常由 RedisBloom 等模块提供，不是 Redis 核心内置的基础数据类型。

## Redis 内存淘汰策略有哪些？

策略可按是否只淘汰设置过期时间的 key、以及淘汰算法区分。常见策略包括 `noeviction`、`allkeys-lru`、`allkeys-lfu`、`allkeys-random`、`volatile-lru`、`volatile-lfu`、`volatile-random`、`volatile-ttl`。默认策略与 Redis 版本和配置有关，回答时注明版本或以运行配置为准。

## Pipeline 是什么？有什么限制？

Pipeline 将多条命令批量发送，减少网络往返。它本身不提供事务原子性；服务端需要缓存批次响应，因此批量过大会增加客户端和服务端内存压力，应分批执行。

## 如何区分缓存穿透、击穿和雪崩？

- **穿透**：请求的数据缓存和数据库都不存在。可做参数校验、缓存空值或使用 Bloom Filter。
- **击穿**：热点 key 失效，集中请求同时落到数据库。可用互斥重建、逻辑过期或热点保护。
- **雪崩**：大量 key 同时过期或缓存服务不可用，造成数据库压力骤增。可错开过期时间、部署高可用、限流降级并准备恢复策略。

## Redis 如何实现分布式锁？

获取锁可用 `SET key token NX PX ttl`，token 应唯一。释放锁时必须原子地比较 token 并删除，通常用 Lua 脚本，避免误删其他客户端的锁。还需考虑租约过期、业务执行超时、续租、故障转移和 fencing token 等问题。

## RDB 与 AOF 有什么区别？

RDB 保存某一时刻的数据快照，文件紧凑、恢复较快，但两次快照之间可能丢失数据。AOF 记录写命令日志，数据恢复精度受刷盘策略影响，通常文件更大。Redis 可配置混合持久化；不能笼统说所有 4.0+ 默认都同时开启。

## Redis 达到 maxmemory 后会怎样？

行为取决于淘汰策略：配置淘汰策略时，写入可能通过淘汰 key 腾出空间；`noeviction` 下可能拒绝会增加内存的写命令。读命令通常仍可执行，但还要考虑复制缓冲区、碎片和实际内存限制。

## 原始笔记中的 Redis 参考链接

- [Redis 面试整理一](https://zhuanlan.zhihu.com/p/594544077)
- [Redis 面试整理二](https://zhuanlan.zhihu.com/p/91539644)
- [Sorted Set 相关文章](https://blog.csdn.net/weixin_37598243/article/details/127625454)
