# 2.3 HTTP 优化指南

解决完DNS的问题后，我们继续看HTTP服务可以做哪些方面的优化，HTTP服务优化有以下几个思路。

- 包体优化：传输数据的包体大小与传输耗时成正相关，压缩算法是减小包体的最有效手段(没有之一)。
- HTTP协议升级：升级至 HTTP/2，进一步可以尝试使用 QUIC，减小 HTTP 请求连接时间。

我们先开始进行包体优化。