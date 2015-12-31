# Async

## Gevent

### Question & Answer

*Q: gevent loop 什么时候切换协程?, 跟 pyc 虚拟机一样(执行一定的时间 or 一定数量的 pyc code)来回切换? 还是 io 时才切换?*

**Exp:**

~~~ python
# -*- coding: utf-8 -*-

import time

import gevent
from gevent import monkey
monkey.patch_all()


def eat_time_by_cpu(i):
    _time = time.time()
    for _ in xrange(3000):
        _ = _ ** _
    time.sleep(3)
    print i, _time, time.time()

jobs = [gevent.spawn(eat_time_by_cpu, i) for i in xrange(5)]
gevent.joinall(jobs)

# 输出
0 1445661816.73 --- 1445661828.89
1 1445661819.2 --- 1445661828.89
2 1445661821.63 --- 1445661828.89
3 1445661824.12 --- 1445661829.55
4 1445661826.55 --- 1445661831.89
~~~

**A: 由输出可知, 即便是阻塞耗时操作, `gevent` 不会去切换协程, 直到执行到 monkey_patch 的 `socket`, `time.sleep` 等操作时, 会用相应的 wrap 的非阻塞方法, 这时才发生协程切换.**


