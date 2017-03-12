---
title: HTTP的请求(request)和响应(response)
---

HTTP ，是超文本传输协议 Hypertext Transfer Protocol 的缩写。

### 资源 和 URL

HTTP是浏览器发出的请求，请求的是服务器上的资源(Resource)，对应的每一个资源都有一个URL，也就是 **统一资源定位地址** ，指向这个资源，分两种：

- 静态资源：也就是各种文件，最常见的是静态HTML，但是也可以是PDF，JSON文件等等，例子：http://haoqicat.com/peter.txt
- 动态资源：也就是URL指向的地方不是一个文件，而是一段代码的入口，服务器经过运算后（比如数据库的增删改查等操作），才返回运算结果给客户端。例子：http://haoqicat.com/username 这个可能就是指向动态资源的，后台对应的可能就是一个API

### curl 发请求

```bash
$ curl -X GET "http://haoqicat.com" -v
```
如果看一下 `man curl` 也就是查看 `curl` 的手册，可以看到 `-X` 选项后面是专门用来指定 `HTTP` 方法的。最后的 `-v` 用来显示详情。所以我们才能看到比较详尽的后续输出内容。

### HTTP请求的格式介绍

#####返回内容的头三行

```
* Rebuilt URL to: http://haoqicat.com/
*   Trying 182.92.203.18...
* Connected to haoqicat.com (182.92.203.18) port 80 (#0)
```

上面的输出表示请求正在被发出，首先找到了 `haoqicat.com` 对应的 IP 地址：`182.92.203.18` 。然后去连接 这个 IP 对应的服务器的 80 端口。端口 也是一个重要的术语，一个服务器好比一座大厦，可以有多个大门， 一个端口就是一道门。`80` 端口是 HTTP 协议默认走的端口。

##### 请求头 Header

```
> GET / HTTP/1.1                
> Host: haoqicat.com
> User-Agent: curl/7.51.0
> Accept: */*
```

对照上面输出依次解释

- GET/POST [URL] HTTP/[version]
- Host 代表被请求的主机，也就是 haoqicat.com
- User-Agent 代表用户使用的客户端，我们这里用的是 curl
- Accept 后面指明客户端可以接受的返回资源的类型，* 代表所有类型都接受
- 更多的header字段讲解可以参考[Wikepeida](https://zh.wikipedia.org/wiki/HTTP%E5%A4%B4%E5%AD%97%E6%AE%B5%E5%88%97%E8%A1%A8)

> 当我们用 GET 发请求的时候，一般我们就是想要从服务器上 GET （拿到）一些内容，而不是想去修改服务器数据。POST 正好就是用来修改服务器上的数据的。
>

##### 负载数据 payload

请求头之中可能还包含一个负载数据（ payload ），GET 请求都是不带负载数据的，POST 请求带负载数据。这个挺好理解，POST 方法的请求 都是要改动服务器数据的，当然要在请求中携带数据过去。比如有一个表单 form ，我们填写几项数据，然后一点提交，这个就会发出一个 POST 请求，而我们填写的数据， 就会作为 payload 成为请求的一部分。
