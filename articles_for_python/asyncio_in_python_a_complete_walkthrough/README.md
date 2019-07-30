<p align="center">
   <img width="200" src="async.png">
</p>

## Python中的Async IO: 完整介绍
by Brad Solomon 

### 目录
- 设置开发环境
- Async IO的一万英尺鸟瞰
- 对多线程Python程序的影响
- 为什么GIL还没有被移除
- 为什么在python3中没有删除它
- 如何处理Python GIL


Async IO 是一种并发编程设计，在Python中得到了专门的支持，从Python 3.4到3.7，得到了快速的发展，甚至[可能到更高的版本](https://twitter.com/1st1/status/1041855365745455104)。

你可能会觉得恐惧，“并发，并行，线程，多处理”。已经很多需要掌握了。异步IO适合哪里？“

本教程旨在帮助你回答这个问题，给你更牢固地掌握Python async IO方法。

下面你将学习到：

- 异步IO(async IO): 一种与语言无关的范例(模型)，它具有跨许多编程语言的实现
- async/await: 用于定义coroutines的两个Python关键字
- asyncio: Python包，提供基础与API用于运行与管理coroutines

coroutines（专用生成器函数）是Python中异步IO的核心，稍后我们将深入研究它们。

**注意：在本篇文章中，我使用术语async IO来表示与语言无关的异步IO设计，而asyncio指的是Python包。**

在开始之前，需要确保已经设置本指南所使用的asyncio和其它库。

### 设置开发环境

你需要Python 3.7或更高版本来完整地阅读本文，以及aiohttp和aiofiles包。

```shell
$ python3.7 -m venv ./py37async
$ source ./py37async/bin/activate  # Windows: .\py37async\Scripts\activate.bat
$ pip install --upgrade pip aiohttp aiofiles  # Optional: aiodns
```
有关安装Python 3.7和设置虚拟环境的帮助，参考 [Python 3 Installation & Setup Guide](https://realpython.com/installing-python/) 或者 [Virtual Environments Primer](https://realpython.com/python-virtual-environments-a-primer/)。

好了，我们开始吧。

### Async IO的一万英尺鸟瞰

Async IO相对于它久经考验的表亲(多处理和线程)来说，不太为人所知。本节将更全面地介绍异步IO是什么，以及它如何适应周围的环境。

#### 异步IO适合哪些场景？

并发(Concurrency)和并行（Parallelism)是扩展的主题，不容易涉及。虽然本文关注的是async IO及其在Python中的实现，但是花点时间将async IO与它的对应对象进行比较是值得的，以便了解async IO如何适应更大的、有时令人眼花缭乱的难题。

并行（Parallelism）包括同时执行多个操作。多处理（Multiprocessing）是一种实现并行的方法，它需要将任务分散到计算机的中央处理单元(cpu或核心)上。多处理非常适合计算密集型(CPU-bound)的任务: 紧密绑定for循环和数学计算通常属于这一类。

并发(Concurrency)是一个比并行（Parallelism)稍微宽泛的术语。它表明多个任务能够以重叠的方式运行。(有句话说并发并不意味着并行。)

线程（Threading）是一个并发执行模型，多个[线程](https://en.wikipedia.org/wiki/Thread_(computing))轮流执行任务。一个进程可以包含多个线程。由于其[GIL](../GIL/README.md), Python与线程有着复杂的关系，但这超出了本文的范围。

了解线程的重要之处是，它更适合于I/O密集型(IO-bound)的任务。计算密集型(CPU-bound)任务的特点是计算机的核心从开始到结束都在不停地工作，而I/O密集型(IO-bound)任务则是由大量等待IO完成。