# RPC、Protocol Buffers 与 gRPC

## RPC 是什么？

远程过程调用让调用方以类似本地调用的编程模型访问远端服务，但底层仍需处理序列化、网络传输、超时、重试、鉴权、服务发现和错误语义。

## Protocol Buffers 为什么常比 JSON 紧凑？

Protocol Buffers 使用 schema 和二进制编码，字段编号可减少字段名重复传输，通常比文本 JSON 更紧凑、解析开销更低。实际效果取决于数据结构、编码方式和压缩；JSON 更易读且生态广。

## gRPC 有哪些特点？

gRPC 通常使用 Protocol Buffers 定义服务和消息，并基于 HTTP/2 提供双向流、多路复用等能力，适合服务间通信。它并非在所有场景都比 HTTP/JSON 更快，还需评估客户端兼容性、调试性、网关和部署约束。
