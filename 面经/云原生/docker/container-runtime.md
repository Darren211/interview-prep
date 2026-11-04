# Docker 与容器运行时

## overlay2 是什么？

overlay2 是 Linux 上常用的 OverlayFS 存储驱动，通过只读镜像层和容器可写层组合文件视图。需要关注写时复制带来的性能特征、镜像层设计和底层文件系统支持。现代部署还应区分 Docker Engine、containerd、CRI 与具体存储驱动的关系。

## 参考资料

- [overlay2 相关文章](https://blog.csdn.net/qq_15770331/article/details/96699386)
