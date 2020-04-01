---
title: socket example api cmds-page (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'api cmds-page'

Functions in program: 
* `def example_decorator(fn):`

## python api cmds-page

Python socket example: api cmds-page

```python
from functools import wraps

from flask_socketio import emit

from api_cmds.base import FlaskView
from cmds.base import route


def example_decorator(fn):
    @wraps(fn)
    def _decorator(*args, **kwargs):
        return fn(*args, 'value', **kwargs)
    
    return _decorator


class Controller(FlaskView):
    route_base = '/example'
  
    def normal(self):
        return "This path can be accessed via `/example/normal` due to FlaskClassy."
    
    @route(path='/routeA')
    def route_decorator(self):
        return "This path can be accessed via `/example/routeA` due to the route decorator."
    
    @route(methods=['POST'])
    @example_decorator
    def route_decorator_default_to_function_name(self, param):
        """This is especially needed, when you have other decorators on this function,
        otherwise, FlaskClassy's routing won't work.
        
        I've also noticed that the decorator can't be two layers deep (eg. a decorator that takes input,
        to create a dynamic decorator). I'm not sure why for now.
        """
        return "This path can be accessed via `/example/route_decorator_default_to_function_name` (POST only)."
    
    @FlaskView.on_socket_event('join')
    def socket_can_be_called_anything_as_long_as_it_is_prefixed_by_socket(self, data):
        """
        :param data: anything that is sent via JS socket.io.
                     In this example, it's going to be `2`, then `{"a": 1}`
        """
        emit('event_name', 'sockets can also emit values')
        emit('broadcast_event', 'this message to everyone', broadcast=True)
        return 'sockets can return values'

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
