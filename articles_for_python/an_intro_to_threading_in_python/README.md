<p align="center">
   <img width="200" src="threading.png">
</p>

## Python中的线程

### 目录
- 什么是线程
- 启动线程
    - 守护线程
    - join()一个线程
- 使用多线程
- 使用ThreadPoolExecutor
- 竞态条件
    - 单个线程
    - 两个线程
    - 为什么这不是一个愚蠢的例子
- 使用锁完成基本同步
- 死锁
- 生产者-消费者线程
    - 生产者-消费者使用锁
    - 生产者-消费者使用Queue
- 线程对象
    - Semaphore
    - Timer
    - Barrier
- 总结：Python中的线程


Python线程允许同时运行程序的不同部分，并可以简化设计。如果你对Python有一定的经验，并且希望使用线程加快程序的速度，那么本教程就是为你准备的！

在本文中，你将学习到：
- 什么是线程？
- 如何创建线程并等待它们完成
- 如何使用ThreadPoolExecutor
- 如何防止竞态条件
- 如何使用Python线程提供的常用工具

本文假设你已经具有Python基础知识，并且至少使用了3.6版本运行示例。如果你需要复习，可以从[Python Learning Paths](https://realpython.com/learning-paths/)开始，快速进入状况。

如果你不确定是否要使用Python线程，asyncio或multiprocessing，那么你可以查看[Speed Up Your Python Program With Concurrency](https://realpython.com/python-concurrency/)。

### 什么是线程?

线程是一个独立的执行流。这意味着你的程序将同时发生两件事。但是对于大多数Python3实现来说，不同的线程实际上并不是同时执行的:它们只是看起来是同时执行的。

很容易将线程视为在程序上运行两个（或更多）不同的处理器，每个处理器同时执行一项独立的任务。 这几乎是正确的。 线程可能在不同的处理器上运行，但它们一次只运行一个。

同时运行多个任务需要一个非标准的Python实现，用不同的语言编写部分代码，或者使用带有一些额外开销的**multiprocessing**。

由于CPython实现Python的工作方式，线程可能无法加速所有任务。这是因为与[GIL](https://realpython.com/python-gil/)的交互本质上限制了一次运行一个Python线程。

花费大量时间等待外部事件的任务通常是线程化的良好候选者。需要大量CPU计算并且花费很少时间等待外部事件的问题可能根本不会运行得更快。

这适用于用Python编写并运行在使用标准CPython实现的代码。如果线程是用C语言编写的，那么它们就能够释放GIL并同时运行。 如果代码运行在不同的Python实现，请查看文档，了解它如何处理线程。

如果你正在运行标准的Python实现，只使用Python编写，并且遇到计算密集型问题，则应该检查**multiprocessing**模块。

构建程序以使用线程也可以提高设计清晰度。你将在本教程中学习的大多数示例，因为使用线程，它们都不一定会运行得更快。在它们中使用线程有助于使设计更清晰、更容易推理。

那么，让我们停止谈论线程，开始使用它！

### 启动线程

既然你已经知道了线是什么，让我们来学习如何制作一个。Python标准库提供了[threading](https://docs.python.org/3/library/threading.html)，它包含了你将在本文中看到的大部分基本类型。在这个模块中，Thread很好地封装了线程，提供了一个清晰的接口来使用它们。

要启动一个单独的线程，你需要创建一个thread实例，然后告诉它.start()。

```python
import logging
import threading
import time

def thread_function(name):
    logging.info("Thread %s: starting", name)
    time.sleep(2)
    logging.info("Thread %s: finishing", name)

if __name__ == "__main__":
    format = "%(asctime)s: %(message)s"
    logging.basicConfig(format=format, level=logging.INFO,
                        datefmt="%H:%M:%S")

    logging.info("Main    : before creating thread")
    x = threading.Thread(target=thread_function, args=(1,))
    logging.info("Main    : before running thread")
    x.start()
    logging.info("Main    : wait for the thread to finish")
    # x.join()
    logging.info("Main    : all done")
```

如果查看日志语句，可以看到主部分正在创建和启动线程。

```python
x = threading.Thread(target=thread_function, args=(1,))
x.start()
```

创建线程时，将传递一个函数和一个包含该函数参数的列表。在本例中，你告诉线程运行thread_function()并将1作为参数传递。

对于本文，你将使用顺序整数作为线程的名称。有一个threading.get_ident()方法，它为每个线程返回一个惟一的名称，但是这些名称通常既不短也不容易读。

thread_function()本身没有多大作用。它只是记录一些消息，其中包含time.sleep()。

当你按原样运行这个程序时(注释掉第20行)，输出将如下所示：
```shell
$ ./single_thread.py
Main    : before creating thread
Main    : before running thread
Thread 1: starting
Main    : wait for the thread to finish
Main    : all done
Thread 1: finishing
```

您将注意到，线程在代码的主部分完成之后才结束。你将会再次回到这里细究原因，在下一节中讨论神秘的第20行。

### 守护线程

在计算机科学中，[守护进程](https://en.wikipedia.org/wiki/Daemon_(computing))是在后台运行的进程。

Python线程对守护进程具有更具体的含义。程序退出时，守护线程将立即关闭。思考这些定义的一种方法是将守护线程视为在后台运行的线程，而不必担心将其关闭。

如果程序运行的线程不是守护线程，那么程序将等待这些线程完成后再终止。但是，如果线程是守护线程，当程序退出时，无论守护线程是什么状态都会被kill。

让我们更仔细地看一下上面程序的输出。最后两行是有趣的一点。当你运行程序时，你会注意到在__main__打印完*all done*之后和线程完成之前有一个暂停（约2秒）。

这个暂停是Python等待非守护线程完成。当Python程序结束时，关闭过程的一部分是清理线程例程(threading routine)。

如果查看[Python线程的源代码](https://github.com/python/cpython/blob/df5cdc11123a35065bbf1636251447d0bfe789a5/Lib/threading.py#L1263)，你会看到threading._shutdown()遍历所有正在运行的线程，并在没有设置**daemon**标志的每个线程上调用.join()。

所以你的程序等待退出，因为线程本身在睡眠中等待。一旦完成并打印了消息，.join()将返回，程序退出。

通常，这种行为是你想要的，但我们还有其他选择。让我们首先使用守护线程重复该程序。你可以通过更改线程的构造方式，添加**daemon=True**标志来实现这一点：

```python
x = threading.Thread(target=thread_function, args=(1,), daemon=True)
```

现在运行程序，你应该看到以下输出：

```shell
$ ./daemon_thread.py
Main    : before creating thread
Main    : before running thread
Thread 1: starting
Main    : wait for the thread to finish
Main    : all done
```

这里的不同之处在于缺少输出的最后一行。thread_function()没有机会完成。它是一个守护线程，所以当__main__到达其代码的末尾并且程序想要完成时，该守护线程被终止。


### join()线程

守护程序线程很方便，但是当你想等待一个线程停止呢？当你想要这样做而不退出你的程序呢？现在让我们回到你的原始程序，看看第二十条注释掉的内容：

```python
# x.join()
```
要让一个线程等待另一个线程完成，可以调用.join()。如果取消对该行的注释，主线程将暂停并等待线程x完成运行。

你是否使用守护线程或普通线程在代码上测试了此功能?结果证明这无关紧要。如果使用.join()线程，则该语句将一直等待，直到任何一种线程完成。
