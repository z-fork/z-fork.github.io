---
title: gevent - time
layout: post
tags:
- python
- gevent
- time
- shell
---

#### 一个 server

**Server:**

~~~ python

# -*- coding: utf-8 -*-

import time

import tornado.ioloop
import tornado.web


class Mainhandler(tornado.web.RequestHandler):

    @tornado.web.asynchronous
    def get(self):
        timestamp = str(time.time())
        self.write(timestamp + '  -  ' + str(time.time()))
        self.finish()


application = tornado.web.Application([
    (r'/', Mainhandler),
])

if __name__ == '__main__':
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()
~~~

#### 测试方法

`/usr/bin/time -l python client`

**Client I**

~~~ python
# -*- coding: utf-8 -*-

from gevent import monkey
monkey.patch_all()

import gevent
import requests


def test():
    requests.get('http://localhost:8888')


times = 5000

jobs = [gevent.spawn(test) for __ in xrange(times)]
gevent.joinall(jobs)
~~~

**Client II**

~~~ python
# -*- coding: utf-8 -*-

import requests


def test():
    requests.get('http://localhost:8888')


times = 5000

for _ in xrange(times):
    test()
~~~

#### 测试结果

~~~ nohighlight

> /usr/bin/time -l python test.py
       14.65 real         8.49 user         1.35 sys
  12029952  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
     13584  page reclaims
         5  page faults
         0  swaps
        65  block input operations
        35  block output operations
     35121  messages sent
     35121  messages received
         0  signals received
     25161  voluntary context switches
     30455  involuntary context switches

> /usr/bin/time -l python test.py
       13.39 real        11.04 user         2.17 sys
 410537984  maximum resident set size
         0  average shared memory size
         0  average unshared data size
         0  average unshared stack size
    110913  page reclaims
        69  page faults
         0  swaps
         6  block input operations
         6  block output operations
     35065  messages sent
     35065  messages received
         0  signals received
     14275  voluntary context switches
    194598  involuntary context switches
~~~

*Q: 差异都表示什么, 因这些差异使用上由什么需要注意的么?*