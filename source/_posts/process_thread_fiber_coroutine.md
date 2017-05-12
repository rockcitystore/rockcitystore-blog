---
title: 进程  LWP 线程 Fiber 协程
---
### 区别


### Process 进程
https://en.wikipedia.org/wiki/Process_(computing)

### Child process 子进程
由父进程生成的进程，称为`Child process`子进程。有时也称为 `subprocess` 或者 `subtask`。


- Children created by fork
    `fork`是一个进程创建自身副本的操作。 它通常是在`kernel`中实现的`system call`。 `fork`是在类Unix操作系统上的主要的创建进程的方法。

- Children created by spawn

    [(preferred in the modern (NT) kernel of Microsoft Windows,](https://en.wikipedia.org/wiki/Child_process)

    [The posix_spawn(3p) and its sibling posix_spawnp can be used as replacements for fork and exec, but does not provide the same flexibility as using fork and exec separately. They may be efficient replacements for fork and exec, but their purpose is to provide process creation primitives in embedded environments where fork is not supported due to lack of dynamic address translation.](https://en.wikipedia.org/wiki/Spawn_(computing)#POSIX_spawn_functions)
 
- Zombie process 僵尸进程 
http://www.cnblogs.com/Anker/p/3271773.html
僵尸进程：一个进程使用fork创建子进程，如果子进程退出，而父进程并没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中。这种进程称之为僵死进程。

    任何一个子进程(init除外)在exit()之后，并非马上就消失掉，而是留下一个称为僵尸进程(Zombie)的数据结构，等待父进程处理。如果进程不调用wait / waitpid的话， 那么保留的那段信息就不会释放，其进程号就会一直被占用，但是系统所能使用的进程号是有限的，如果大量的产生僵死进程，将因为没有可用的进程号而导致系统不能产生新的进程. 此即为僵尸进程的危害，应当避免。

- Orphan process 孤儿进程
http://www.cnblogs.com/Anker/p/3271773.html
孤儿进程是没有父进程的进程，孤儿进程这个重任就落到了init进程身上，init进程就好像是一个民政局，专门负责处理孤儿进程的善后工作。每当出现一个孤儿进程的时候，内核就把孤 儿进程的父进程设置为init，而init进程会循环地wait()它的已经退出的子进程。这样，当一个孤儿进程凄凉地结束了其生命周期的时候，init进程就会代表党和政府出面处理它的一切善后工作。**因此孤儿进程并不会有什么危害。**

### Thread 线程
### Green Thread
### Fiber
### Coroutine 协程
