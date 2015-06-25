---
title: 使用gdb解析core dump-概念篇
date: '2015-06-06'
description:
categories:
- coredump
tags:
- coredump
---

### Core dump是什么？

当一个程序在运行的过程中发生了错误，导致**程序崩溃**或者**异常终止**的时候，操作系统会将这个程序在问题发生时的一些信息以文件的形式保存起来，以便维护人员进行调查，解析问题发生的原因，这个文件就是Core dump文件。
从字面上理解，Core dump（核心转储）就是将core信息以dump格式保存在文件中。
Core信息包括了进程信息、内存信息、寄存器信息、堆栈信息等。

### Core dump保存在哪里？

**开启core dump功能**

要想获得Coredump文件就必须先开启保存该文件的功能。
使用`ulimit -c`可以查看是否开启。如果输出0说明没有开启，使用`ulimit -c unlimited`可以开启，这种方式不会限制core文件的大小；使用`ulimit -c  size`可以设置core文件大小，单位是KB。但是这种方式只能设置当前终端有效，如果希望永久开启，可以在`/etc/profile`中追加`ulimit -c unlimited`。

**修改core文件位置**

默认情况下会在程序所在目录生成文件名为`core`的core文件；
使用`echo 1 > /proc/sys/kernel/core_uses_pid`可以在生成带进程PID的core文件，文件形式为`core.pid`；
使用`echo "/tmp/core/core-%e.%p" > /proc/sys/kernel/core_pattern`可以在`/tmp/core`中生成格式为`core-程序名.pid`的core文件。

### Core dump如何产生？

我们知道了程序在**程序崩溃**或者**异常终止**的时候会产生core文件呢？那么使用`kill -9`会产生core文件吗？
我们不妨做个试验。我们给helloword程序加上死循环来进行测试。

```
#include <stdio.h>

int main(int argc, char **argv)
{
    while(1){
        fprintf(stdout,"Hello World!\n");
        fflush(stdout);
        sleep(5);
    }
    return 0;
}
```
```
$ gcc hello.c -o hello
$ ./hello
Hello World!
Hello World!
$ ps -ef | grep hello
archerd+  2890  2044  0 20:55 pts/1    00:00:00 ./hello
```
接下来我们使用下面的命令结束进程，发现没有生成core文件。
> pkill -9 hello

我们继续使用下面的命令结束进程，发现产生core文件。
> kill -s 11 \`pgrep hello`

我们可以得出一个结论，并非所有的中断信号都产生core文件。那么什么信号会产生core文件呢？？
我们使用`man 7 signal`来查看一下,原来下表中Action为core的信号才会产生core文件。

```
Signal     Value     Action   Comment
──────────────────────────────────────────────────────────────────────
SIGHUP        1       Term    Hangup detected on controlling terminal
                              or death of controlling process
SIGINT        2       Term    Interrupt from keyboard
SIGQUIT       3       Core    Quit from keyboard
SIGILL        4       Core    Illegal Instruction
SIGABRT       6       Core    Abort signal from abort(3)
SIGFPE        8       Core    Floating point exception
SIGKILL       9       Term    Kill signal
SIGSEGV      11       Core    Invalid memory reference
SIGPIPE      13       Term    Broken pipe: write to pipe with no
                              readers
SIGALRM      14       Term    Timer signal from alarm(2)
SIGTERM      15       Term    Termination signal
SIGUSR1   30,10,16    Term    User-defined signal 1
SIGUSR2   31,12,17    Term    User-defined signal 2
SIGCHLD   20,17,18    Ign     Child stopped or terminated
SIGCONT   19,18,25    Cont    Continue if stopped
SIGSTOP   17,19,23    Stop    Stop process
SIGTSTP   18,20,24    Stop    Stop typed at terminal
SIGTTIN   21,21,26    Stop    Terminal input for background process
SIGTTOU   22,22,27    Stop    Terminal output for background process
```

