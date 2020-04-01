---
title: socket example b (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'b'

Functions in program: 
* `def test():`

Modules used in program: 
* `import concurrent.futures.thread`
* `import tornado.ioloop`
* `import tornado.web`
* `import tornado.wsgi`
* `import tornado`
* `import traceback`
* `import bottle`
* `import time`

## python b

Python socket example: b

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import time
import bottle
import traceback
import tornado
import tornado.wsgi
import tornado.web
import tornado.ioloop
import concurrent.futures.thread

wapp = bottle.Bottle()
instance = tornado.ioloop.IOLoop.instance()

@wapp.get('/test')
def test():
    return """
    Hello World
    """

class WSGIHandler(tornado.web.RequestHandler):

    executor = concurrent.futures.thread.ThreadPoolExecutor(max_workers=1)
    count = 0
    
    def initialize(self, fallback, threaded=False):
        self.fallback = fallback
        self.threaded = threaded
        self._error = False

    def prepare(self):
        self.count += 1
        if self.threaded:
            print('req <= {}'.format(self.count))
            self.ft = self.executor.submit(self.fallback, self.request)
            self.ft.add_done_callback(self.ft_finish)
            q = self.executor._work_queue
            print("queue size: {} task_handling: {}".format(len(q.queue), q.unfinished_tasks))

    def ft_finish(self, ft):
        try:
            # this have to have timeout because without some browser requests are not finished
            ft.result(60)
            # this will close cleanup tornado part
            self.request.finish()
            self._finished = True
            self.on_finish()
            # self._break_cycles()
        except Exception as e:
            self._error = True
            print(traceback.format_exc())
            # raise e.__class__, e, ft._traceback
        finally:
            pass

    def on_finish(self):
        self.count += 1
        print('req => {}'.format(self.count))

    def post(self, *args, **kwargs):
        return self.ft

    def get(self, *args, **kwargs):
        """ See WSGIHandler.post """
        return self.ft

    def put(self, *args, **kwargs):
        """ See WSGIHandler.post """
        return self.ft

    def delete(self, *args, **kwargs):
        """ See WSGIHandler.post """
        return self.ft

    def req_finish(self):
        if self._error:
            print(self.ft._exception)
        else:
            self._finished = True

    def send_error(self, status_code=500, **kwargs):
        self.req_finish()
        if not self._finished:
            self.finish()

    def write_error(self, status_code, **kwargs):
        tornado.web.RequestHandler.write_error(self, status_code, **kwargs)

if __name__ == '__main__':
    fb = tornado.wsgi.WSGIContainer(wapp)
    app = tornado.web.Application(
        handlers=[('.*', WSGIHandler, dict(fallback=fb, threaded=True))],
        default_host=None,
        transforms=None)
    web = app
    web.listen(5001, address="0.0.0.0")
    instance.start()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
