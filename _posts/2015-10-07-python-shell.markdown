---
title: python shell
layout: post
tags:
- python
- shell
---

#### module

{% highlight python %}
os, commands, subprocess
{% endhighlight %}

#### function

* **os.execl(path, \*args)**

`execl` 与 Unix 的 exec 系统调用一致. 这些方法适用于子进程中调用外部程序的情况, 外部程序会替换当前进程代码, 不会返回.

    os.execl('/usr/bin/python', 'python', '--version')

* **os.system(command)**

`system` 会创建子进程运行 command 命令, 并返回 command 命令执行完成后的推出状态. *实际上是适用 c 标准库函数 system()实现*. 这个方法适用没有输出结果只关心是否正常运行退出.

    assert 0 == os.system('ls /bin/ls')

* **os.popen(command, mode, bufesize)**

`popen` 打开一个与 command 进程之间的管道, 获取外部程序的输出结果, 返回一个 file 对象.

    import os
    p = os.popen('ls')
    assert isinstance(p, file) is True

* **commands.getstatusoutput(command)**

`getstatusoutput` 使用 `os.popen()` 执行 command 命令并返回一个执行状态和执行结果的tuple (status, output). 实际上以 *command 2>&1* 方式执行, output 中包含 stdout, stderr. output 中不包含尾部换行符.

    import commands
    assert (0, '/bin/ls') == commands.getstatusoutput('ls /bin/ls')

**This module intends to replace serveral other, older modules and functions, shuch as: os.system, os.spawn\*, os.popen\*, popen2.\*, commands.\***

* **subprocess.call(command)**

如果 command 不是一个可执行文件, shell=True.

    import subprocess
    assert 0 == subprocess.call('ls /bin/ls', shell=True)

* **subprocess.Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0)**

    args                - str or list or tuple, 用于指定进程的可执行文件及其参数. 如果是 list or tuple 第一个元素通常是可执行文件的路径. 也可以显示的在 executable 参数来指定可执行文件的路径.
    bufsize             -
    executable          - 用于指定可执行程序. 如果将参数 shell 设为 True, executable 将指定程序使用的 shell.
    stdin               - 标准输入, 可以是PIPE, 文件描述符, 文件对象. 也可以设置为None, 表示从父进程继承.
    stdout              - 标准输出, 可以是PIPE, 文件描述符, 文件对象. 也可以设置为None, 表示从父进程继承.
    stderr              - 标准错误, 可以是PIPE, 文件描述符, 文件对象. 也可以设置为None, 表示从父进程继承.
    preexec_fn          - 用于指定一个可执行对象(callable object), 将在子进程运行之前被调用.
    close_fds           -
    shell               - 如果参数 shell 设为 True, 程序将通过 shell 来执行.
    cwd                 - 用于设置子进程的当前目录.
    env                 - dict类型, 用于指定子进程的环境变量. 如果 env = None, 子进程的环境变量将从父进程中继承.
    universal_newlines  - 换行符, 如果将此参数设置为 True, Python 统一换行符当作 '\n' 来处理.
    startupinfo         -
    creationflags       -

`subprocess.PIPE`

subprocess.PIPE 用于初始化 stdin, stdout, stderr 参数. 表示与子进程通信的标准流.

`subprocess.STDOUT`

subprocess.STDOUT 用于初始化 stderr 参数. 表示将错误通过标准输出流输出.

`poll()`

用于检查子进程是否已经结束, return code.

`wait()`

等待子进程结束, return code.

`communicate(input=None)`

与子进程进行交互. 向 stdin 发送数据, 或从 stdout 和 stderr 中读取数据. 可选参数 input 指定发送到子进程的参数. communicate() 返回一个tuple, (stdoutdata, stderrdata). *如果希望通过 stdin 向其发送数据, 创建 Popen 对象的时候, 参数 stdin 必须被设置为 PIPE. stdout, stderr 同上*.

`send_signal(signal)`

向子进程发送信号.

`terminate()`

停止子进程.

`kill()`

杀死子进程

`stdin, stdout, stderr`

如果创建 Popen 对象时, 参数 stdin, stdout, stderr 设置为 subprocess.PIPE; stdin, stdout, stderr 返回一个文件对象用于给子进程发送指令. 否则返回 None.

`pid`

获取子进程的进程ID.

`returncode`

获取子进程的返回值. 如果进程还没结束, 返回 None.

#### example

    import time
    import signal
    import tempfile
    import subprocess


    def run(command):
        process = subprocess.Popen(command, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, shell=True)
        output, _ = process.communicate()
        return process.returncode, output


    def run_async(command, timeout=5):
        tf = tempfile.TemporaryFile()
        process = subprocess.Popen(command, stdout=tf, stderr=subprocess.STDOUT, shell=True)
        seek = 0
        start_time = time.time()
        while True:
            time.sleep(0.1)

            tf.seek(seek)
            line = tf.readline()

            if not line:
                if process.poll() is None:
                    continue
                else:
                    break
            else:
                if time.time() - start_time > timeout:
                    process.send_signal(signal.SIGTERM)
                seek = tf.tell()

            yield line


    print run('ls /bin/ls')


    for message in run_async('ping www.github.com', 5):
        print message

#### todo

* [python sh](https://github.com/amoffat/sh)
