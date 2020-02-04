# TLS/SSL/HTTPS

> References:
>
> [https://blog.csdn.net/enweitech/article/details/81781405](https://blog.csdn.net/enweitech/article/details/81781405)
>
> [https://www.jianshu.com/p/1fc7130eb2c2](https://www.jianshu.com/p/1fc7130eb2c2)

SSL\(Secure Socket Layer 安全套接层\)是基于HTTPS下的一个协议加密层，最初是由网景公司（Netscape）研发，后被IETF（The Internet Engineering Task Force - 互联网工程任务组）标准化后写入（RFCRequest For Comments 请求注释），RFC里包含了很多互联网技术的规范！

起初是因为HTTP在传输数据时使用的是明文（虽然说POST提交的数据时放在报体里看不到的，但是还是可以通过抓包工具窃取到）是不安全的，为了解决这一隐患网景公司推出了SSL安全套接字协议层，SSL是基于HTTP之下TCP之上的一个协议层，是基于HTTP标准并对TCP传输数据时进行加密，所以HPPTS是HTTP+SSL/TCP的简称。

由于HTTPS的推出受到了很多人的欢迎，在SSL更新到3.0时，IETF对SSL3.0进行了标准化，并添加了少数机制\(但是几乎和SSL3.0无差异\)，标准化后的IETF更名为TLS1.0\(Transport Layer Security 安全传输层协议\)，可以说TLS就是SSL的新版本3.1，并同时发布“RFC2246-TLS加密协议详解”，如果想更深层次的了解TLS的工作原理可以去RFC的官方网站：www.rfc-editor.org，搜索RFC2246即可找到RFC文档。

#### TLS认证证书

TLS认证证书可以理解为TLS的一种实现方式，一般，认证证书分成以下几种：

1. 自签证书，

