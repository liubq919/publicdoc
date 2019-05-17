## 章节2：解决方案探讨

#### websocket

websocket的实现方案有很多种，这个主要看自己的个人选择，只要能在功能和协议上达到目标。

在本方案中，我们选择了[tornado](https://www.tornadoweb.org/)，一个python web框架： 

1. tornado由于使用的是单线程非堵塞的方式处理请求，因此性能显著高于传统模式下的框架；
2. 最新版本的tornado，v6，使用了python3中的很多关键字，降低学习成本；
3. 模块成熟，可以解决大部分的使用场景

#### 主机端口扫描

[masscan](https://github.com/robertdavidgraham/masscan)，强大的ip，端口扫描工具
