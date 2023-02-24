<p align="center">
   <img width="200" src="gil.png">
</p>

## 什么是Python全局解释器锁(GIL)?

### 目录
- GIL为Python解决了什么问题
- 为什么选择GIL作为解决方案
- 对多线程Python程序的影响
- 为什么GIL还没有被移除
- 为什么在python3中没有删除它
- 如何处理Python GIL

简单地说，Python全局解释器锁或GIL是一个互斥锁(或锁)，它只允许一个线程控制Python解释器。

这意味着在任何时间点都只有一个线程处于执行状态。GIL的影响对于执行单线程程序的开发人员是不可见的，但是对于计算密集型（CPU-bound）和多线程代码来说，它可能是性能瓶颈。

由于GIL一次只允许执行一个线程，即使在具有多核CPU内核的多线程架构中也是如此，因此GIL作为Python的一个臭名昭著的特性而闻名。

**在本文中，您将了解GIL如何影响Python程序的性能，以及如何减轻它对代码的影响。**

### GIL为Python解决了什么问题

Python使用引用计数来管理内存。这意味着在Python中创建的对象有一个引用计数变量，该变量跟踪指向该对象的引用数量。当该计数达到零时，对象占用的内存被释放。

让我们看一个简短的代码示例，以演示引用计数是如何工作的：
```python
>>> import sys
>>> a = []
>>> b = a
>>> sys.getrefcount(a)
3
```
在上面的例子中，空列表对象[]的引用计数为3。list对象由a、b和传递给sys.getrefcount()的参数所引用。

回到GIL：

问题是这个引用计数变量在竞争条件中需要保护，以避免两个线程同时增加或减少其值。如果发生这种情况，它可能导致永远不会释放的泄漏内存，或者更糟的是，在对该对象的引用仍然存在时不正确地释放内存。这可能会导致Python程序中出现崩溃或其他“怪异”错误。

通过向线程间共享的所有数据结构添加锁，从而避免不一致地修改这些数据结构，可以确保这个引用计数变量的安全性。

但是，为每个对象或对象组添加一个锁意味着存在多个锁，这可能会导致另一个问题死锁(死锁只有在有多个锁时才会发生)。另一个副作用是重复获取和释放锁会降低性能。

GIL是解释器本身上的一个锁，它添加了一条规则，即任何Python字节码的执行都需要获得解释器锁。这可以防止死锁(因为只有一个锁)，并且不会带来太多性能开销。但它实际上使任何计算密集型（CPU-bound）Python程序成为单线程。

GIL，尽管被其它语言的编译器使用，比如ruby，但是它并不是该问题的唯一解决方案。一些语言通过使用引用计数以外的方案，比如垃圾回收，避免了GIL对线程安全内存管理的要求。

另一方面，这意味着这些语言通常需要通过添加其他性能提升功能（如JIT编译器）来弥补GIL单线程性能优势的损失。

### 为什么选择GIL作为解决方案

那么，为什么看起来如此阻塞的方法会在Python中使用呢?这是Python开发人员的一个错误决定吗？

好吧，用Larry Hastings的话来说，GIL的设计决策是使Python今天如此流行的原因之一。

从操作系统还没有线程概念的时候起，Python就已经存在了。Python被设计成易于使用，以便使开发更快，并且越来越多的开发人员开始使用它。

许多扩展是为现有的C语言库编写的，这些库的特性是Python所需要的。为了防止不一致的更改，这些C语言扩展需要GIL提供的线程安全内存管理。

GIL易于实现，并且很容易添加到Python中。它为单线程程序提供了性能提升，因为只需要管理一个锁。

非线程安全的C语言库更容易集成。这些C语言扩展成为Python被不同社区欣然采用的原因之一。

正如您所看到的，GIL是一个实用的解决方案，可以解决CPython开发人员在Python生命中早期面临的一个难题。

### 对多线程Python程序的影响

当你查看典型的Python程序或任何计算机程序时，那些在性能上受CPU限制的程序与I/O密集型的程序之间存在差异。

计算密集型（CPU-bound）程序是那些将CPU推向极限的程序。这包括进行数学计算的程序，如矩阵乘法，搜索，图像处理等。

I/O密集型的程序是花费时间等待输入/输出的程序，这些输入/输出可以来自用户，文件，数据库，网络等。

I/O密集型程序有时必须等待大量时间，直到它们从源获得所需内容，因为源可能需要在输入/输出准备好之前进行自己的处理，例如，a user thinking about what to enter into an input prompt or a database query running in its own process.

我们来看一个执行倒计时的简单计算密集型程序：

```python
# single_threaded.py
import time
from threading import Thread

COUNT = 50000000

def countdown(n):
    while n>0:
        n -= 1

start = time.time()
countdown(COUNT)
end = time.time()

print('Time taken in seconds -', end - start)
```

在具有4个内核的系统上运行这段代码会得到以下输出：

```shell
$ python single_threaded.py
Time taken in seconds - 6.20024037361145
```

现在，我稍微修改了代码，使用两个并行的线程来执行相同的倒计时：

```python
# multi_threaded.py
import time
from threading import Thread

COUNT = 50000000

def countdown(n):
    while n>0:
        n -= 1

t1 = Thread(target=countdown, args=(COUNT//2,))
t2 = Thread(target=countdown, args=(COUNT//2,))

start = time.time()
t1.start()
t2.start()
t1.join()
t2.join()
end = time.time()

print('Time taken in seconds -', end - start)
```

当我再次运行它时：

```shell
$ python multi_threaded.py
Time taken in seconds - 6.924342632293701
```

正如您所看到的，两个版本完成所需的时间几乎相同。在多线程版本中，GIL防止计算密集型线程以并行方式执行。

GIL对I/O密集型的多线程程序的性能影响不大，因为线程在等待I/O时会共享锁。

但是线程完全是计算密集型（CPU-bound）程序，比如，使用线程处理部分图像的程序，不仅会因锁而变成单线程，而且还会看到执行时间的增加，如上例所示，与完全使用单线程编写的场景相比。

该执行时间的提升是获取与释放的开销，此开销由锁添加。

### 为什么GIL还没有被移除？

Python 3确实有机会从头开始许多特性，在这个过程中，它破坏了一些现有的C扩展，然后需要对这些扩展进行更新，并将其移植到Python 3中。这就是Python 3的早期版本被社区接受度慢的原因。

但为什么GIL并没有一起被删除？

与单线程性能的python2相比，删除GIL会使python3的速度变慢，你可以想象这会导致什么结果。你不能否认GIL的单线程性能优势。所以结果是python 3仍然有GIL。

但是Python 3确实对现有GIL进行了重大改进-

我们讨论了GIL对“仅计算密集型”和“仅I/O密集型”多线程程序的影响，但是对那些有些线程是计算密集型，有些是I/O密集型的程序怎么样呢？

在这样的程序中，Python的GIL被熟知以通过不给I/O密集型线程从计算密集型线程中获取GIL的任何机会以饥饿I/O密集型线程。

这是因为Python中内置的一种机制，它强制线程在连续使用固定时间间隔后释放GIL，如果没有其他线程获得GIL，则该线程可以继续使用它。

```python
>>> import sys
>>> # The interval is set to 100 instructions:
>>> sys.getcheckinterval()
100
```

这种机制的问题是，大多数时候计算密集型线程会在其他线程获得GIL之前**重新**获得GIL。David Beazley对此进行了研究，供参考的[可视化](https://www.dabeaz.com/blog/2010/01/python-gil-visualized.html)。

Antoine Pitrou在2009年的Python 3.2中修复了这个问题，他添加了一种机制，可以查看由其它线程的发起的且被丢弃的GIL请求数，在其他线程有机会运行之前不允许当前线程重新获取GIL。


### 如何处理Python的GIL？

如果GIL导致你出现问题，可以尝试以下几种方法：


**多进程与多线程** - 最流行的方法是使用多进程方法，你可以使用多个进程而不是线程。每个Python进程都有自己的Python解释器和内存空间，因此GIL不会成为问题。Python有一个multiprocessing 模块，可以让我们像这样轻松地创建流程：

```python
from multiprocessing import Pool
import time

COUNT = 50000000
def countdown(n):
    while n>0:
        n -= 1

if __name__ == '__main__':
    pool = Pool(processes=2)
    start = time.time()
    r1 = pool.apply_async(countdown, [COUNT//2])
    r2 = pool.apply_async(countdown, [COUNT//2])
    pool.close()
    pool.join()
    end = time.time()
    print('Time taken in seconds -', end - start)
```

在我的系统上运行此命令给出了此输出：

```shell
$ python multiprocess.py
Time taken in seconds - 4.060242414474487
```

与多线程版本相比，性能有了不错的提升，对吧。

时间没有下降到我们上面看到的一半，因为进程管理有自己的开销。多个进程比多个进程重，因此请记住，这可能会成为一个扩展瓶颈。

**Python解释器的替代** - Python有多个解释器实现。分别用C、Java、c#和Python编写的CPython、Jython、IronPython和PyPy，这些解释器都是是最流行的。GIL只存在于原始的Python实现(即CPython)中。如果你的程序及其库可在其它解释器中使用，那么你也可以试用其它的解释器。

**请静候结果** - 虽然许多Python用户利用了GIL的单线程性能优势。多线程程序员不必烦恼，因为Python社区中一些最聪明的人正在努力从CPython中删除GIL。一个熟知的尝试是[Gilectomy](https://github.com/larryhastings/gilectomy)。

Python GIL通常被认为是一个神秘而困难的话题。但请记住，作为Python支持者，如果你正在编写C扩展或者在程序中使用计算密集型的多线程，那么通常只会在这些情况下才受到GIL的影响。

在这种情况下，本文将为你提供理解GIL是什么以及如何在自己的项目中处理它所需的一切。如果你想了解GIL的底层内部工作原理，我建议你观看David Beazley的[《Understanding the Python GIL》](https://www.youtube.com/watch?v=Obt-vMVdM8s&feature=youtu.be)的演讲。
