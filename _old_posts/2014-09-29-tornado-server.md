---
title: tornado server
layout: post
tags:
  - tornado
  - server

---

*Tornado Server*

官网实例:

    import tornado.ioloop
    import tornado.web

    class MainHandler(tornado.web.RequestHandler):
        def get(self):
            self.write("Hello, world")

    application = tornado.web.Application([
        (r"/", MainHandler),
    ])

    if __name__ == "__main__":
        application.listen(8888)
        tornado.ioloop.IOLoop.instance().start()

单进程处理请求不够给力, 使用Tornado自带的tornado.process

tornado.process 实例:

    import tornado.web
    import tornado.httpserver
    import tornado.ioloop
    import tornado.netutil
    import tornado.process
    from tornado.options import define, options
    import os
    import time

    define("port", default=8001, help="run on the given port", type=int)


    class MainHandler(tornado.web.RequestHandler):
        def get(self):
            self.write("Hello, world")


    class LongHandler(tornado.web.RequestHandler):
        def get(self):
            self.write(str(os.getpid()))
            time.sleep(2)


    if __name__ == "__main__":
        tornado.options.parse_command_line()
        app = tornado.web.Application([
            (r"/", MainHandler),
            (r"/long", LongHandler),
        ])
        sockets = tornado.netutil.bind_sockets(8888)
        tornado.process.fork_processes(0)
        server = tornado.httpserver.HTTPServer(app)
        server.add_sockets(sockets)
        tornado.ioloop.IOLoop.instance().start()

已然万事大吉?

使用过程中难免会因更改需求或修复 bug, 就免不了要重启 Tornado.
如果使用上述方法的话, 会有一个主进程和多个子进程需要 kill, 然后再重新运行.

如果手动一个一个查找pid, kill 掉过于麻烦, 就使用了 supervisor.

已然万事大吉?

使用supervisor启动, stop, restart 主进程的时候, 结果却出错了, Supervisor 认为没有启动成功, 但其实已经重启好了

tornado.process.fork_processes() 方法产生的子进程并不会随主进程的退出而退出, 而 Supervisor 当然是不知道这些子进程的.
( 顺带一提, Supervisor 也不能管理守护进程. ) 如此一来, 如果主进程有什么异常, 或者被 kill 掉了,子进程就变成僵尸进程了.

自己创建多个进程, 分布在多个端口, 然后由 nginx 来反向代理到 80 端口.

    from tornado import web, ioloop
    from tornado import options

    options.define("port", default=8001, help="run on the given port", type=int)


    class MainHandler(web.RequestHandler):
        def get(self):
            self.write("Hello, world")

    if __name__ == "__main__":
        options.parse_command_line()
        port = options.options.port
        app = web.Application([
            (r"/", MainHandler),
        ])
        app.listen(port)
        ioloop.IOLoop.instance().start()

已然万事大吉?

每次重启都会导致网站有大约 10 秒无法访问, 这显然不够好.

依次重启各个进程

    for i in {x..y}
        do supervisorctl -c supervisord.conf restart appname-$i
    done
    nginx -s reload

已然万事大吉?

这样基本任何时候网站都是可访问的, 不过在重启的过程中, 有些没处理完的请求可能会被直接中断掉.

捕捉 TERM 和 INT 信号, 使 Tornado 在退出前先停止接收新请求 (由 nginx 分发到其他端口), 再尝试处理未完成的回调, 最后才退出.

    from tornado import web, ioloop
    from tornado import options
    import time

    options.define("port", default=8001, help="run on the given port", type=int)


    class MainHandler(web.RequestHandler):
        def get(self):
            self.write("Hello, world")

    def sig_handler(sig, frame):
        ioloop.IOLoop.instance().add_callback(shutdown)

    def shutdown():
        # 不接收新的 HTTP 请求
        server.stop()

        io_loop = ioloop.IOLoop.instance()
        deadline = time.time() + settings.MAX_WAIT_SECONDS_BEFORE_SHUTDOWN

        def stop_loop():
            now = time.time()
            if now < deadline and (io_loop._callbacks or io_loop._timeouts):
                io_loop.add_timeout(now + 1, stop_loop)
            else:
                # 处理完现有的 callback 和 timeout 后，可以跳出 io_loop.start() 里的循环
                io_loop.stop()
        stop_loop()

    if __name__ == "__main__":
        options.parse_command_line()
        port = options.options.port
        app = web.Application([
            (r"/", MainHandler),
        ])
        app.listen(port)
        signal.signal(signal.SIGTERM, sig_handler)
        signal.signal(signal.SIGINT, sig_handler)
        ioloop.IOLoop.instance().start()

以这种方式重启, 耗时几秒的请求也不会被强制中断.
100 个并发的压力测试下, 重启过程中也没有任何失败请求出现.

settings.MAX_WAIT_SECONDS_BEFORE_SHUTDOWN 设超过 10 秒,
就要相应地增加 supervisord.conf 中 stopwaitsecs 的时间, 否则会被强行杀掉的.