---
title: 并行计算与并发计算
---

[toc]
##并行计算与并发计算
####概念
- [Parallel_computing](https://en.wikipedia.org/wiki/Parallel_computing)


- Concurrency http://en.wikipedia.org/wiki/Concurrency_(computer_science)

####  [区别与相同点](https://en.wikipedia.org/wiki/Parallel_computing)

并行计算与并发计算密切相关 - 它们经常被一起使用，并且经常混合在一起，尽管两者是截然不同的：

- 相同点:
 可以具有并行性（如位级并行性）和并行性（如并行性）（如多任务） 通过在单核CPU上分时共享）。[5] [6] 

- 区别:  
在并行计算中，计算任务通常在几个，通常很多非常相似的子任务中进行分解，子任务可以在完成后独立处理，**其结果将在之后组合**。 
	相比之下，在并发计算中，各种过程通常不涉及相关任务; 当他们这样做时，如在分布式计算中典型的，单独的任务可能具有不同的性质，并且在执行期间通常需要一些进程间通信。
