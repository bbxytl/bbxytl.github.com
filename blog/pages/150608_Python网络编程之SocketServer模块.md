[GitHubBlog](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
# [Python网络编程之SocketServer模块](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/150608_Python网络编程之SocketServer模块.md#githubblog-)
---

`SocketServer` 模块简化了编写网络服务器的任务。在`Python 3` 中 改名为：`socketserver`。

## 2. 服务器对象
- *class* SocketServer.**BaseServer** :
	- 这是模块中的所有服务器对象的超类。它定义了接口（下面给出），大部分要在子类中去实现的方法。
- BaseServer.**fileno**()
	- 返回服务器正在监听的套接字的整数文件描述符。此函数经常传递给 `select.select()`，以便在相同的进程监视多个服务器。
- BaseServer.**handle_request**()
	- 处理单个请求。此函数会调用下列函数：
		- `get_request()`
		- `verify_request()`
		- `process_request()`
	- 如果用户定义的 `handle()` 方法引发了异常，服务器的 `handle_error()	` 方法将被调用。
	- 如果达到最大秒 `self.timeout` ，仍然没有请求到达，则 `handle_timeout()` 将被调用，函数 `handle_request()` 返回。
- BaseServer.**serve_forever**(*poll_interval=0.5*)
	- 处理请求，直到显式的关闭请求（调用：`shutdown()`）。每 *poll_interval* 秒轮训一次。忽略 `self.timeout` 。如果你需要做的定期任务，需要在另一个线程中进行处理。
- BaseServer.**shutdown**()




## **附录**
- **[博客园-Blog](http://www.cnblogs.com/lomper/)**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)

