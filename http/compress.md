# 2.4.1 对传输内容进行压缩

对传输内容进行压缩是提升HTTP服务效率的重要方式，如果使用 Gzip 压缩，一般一个 100KB 的文件压缩之后会变成 30KB，这将极大减小传输时间。

## 1.HTTP压缩的原理

所有的现代浏览器、客户端及HTTP服务软件都支持压缩技术，唯一需要协商的是所采用的压缩算法。

为了选择采用的压缩算法，HTTP客户端和服务器之间会使用主动协商机制，HTTP客户端发送 Accept-Encoding 首部（其中包含它所支持的压缩算法，以及各自的优先级），服务器则从中选择一种，使用该算法对响应的消息主体进行压缩，并且发送 Content-Encoding 首部来告知HTTP客户端它选择了哪一种算法。

<div  align="center">
	<img src="../assets/compress.png" width = "400"  align=center />
	<p>图 2-8 HTTP 压缩协商流程</p>
</div>

## 2.使用 Brotli 压缩

Brotli 是 Google 推出的开源无损压缩算法，通过变种的 LZ77 算法、Huffman 编码以及二阶文本建模等方式进行数据压缩。Brotli内部有一个预定义的字典。该字典包含超过 13000 个常用单词、短语和其他子字符串，
在这种预定义字典下，在 LZ77 压缩中字典中的一个词会作为一个整体被匹配，这种方式可以大大提升较小文件的压缩密度。


服务端和移动端使用brolti时需要额外安装。服务端安装后可以和 gzip 一同开启，如下为 Nginx 配置示例。

```
http {
	brotli on;
    brotli_comp_level 6;
    brotli_buffers 16 8k;
    brotli_min_length 20;
    brotli_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;	
}
```

Brotli 侧重于HTTP应用类服务内容的压缩，有着更高的压缩效率，性能也比我们目前常见的 Gzip 高 `17~25%`，如图3-7所示。

<div  align="center">
	<img src="../assets/brotli.jpeg" width = "480"  align=center />
	<p>图 2-9 Brotli 压缩效果对比</p>
</div>