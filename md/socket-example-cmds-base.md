---
title: socket example cmds-base (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cmds-base'

Functions in program: 
* `def route(path="", **options):`

Modules used in program: 
* `import flask_classy`

## python cmds-base

Python socket example: cmds-base

```python
import flask_classy


class IgnoredFlaskClassyFunction(Exception):
    pass


class FlaskView(flask_classy.FlaskView):
    # Set default setting for trailing_slash (so routes don't need to end with /)
    trailing_slash = False

    socket_cache_name = '_socket_rule_cache'    # To store socket event listeners

    @classmethod
    def build_rule(cls, rule, method=None):
        """Currently, this is the only way to add custom rules for ignoring
        function names within flask_classy views."""

        if rule.startswith('/socket_'):
            # To support socket functions in the class, we denote this prefix
            # to be purely socket functions.
            # Furthermore, raising this exception is the only way we can get
            # around vendor code of ignoring creating a route.
            raise IgnoredFlaskClassyFunction

        return super(FlaskView, cls).build_rule(rule, method)

    @classmethod
    def socket_shim(cls, function):
        """A shim function is needed, because `socket.on_event` passes all inputs to
        function parameters (ignoring the class' self parameter). Therefore, use
        a shim function to inject this.

        :param function: function
        :return: function
        """
        def callback(*args, **kwargs):
            return function(cls, *args, **kwargs)

        return callback

    @classmethod
    def add_socket_routes(cls, socket):
        """Register socket routes.
        
        :param socket: socketio object.
        """
        methods = getattr(cls, cls.socket_cache_name)
        for function_name in methods:
            function = methods[function_name][0]
            for event_name in methods[function_name][1:]:
                socket.on_event(
                    event_name,
                    cls.socket_shim(function),
                    namespace = cls.route_prefix + cls.route_base,
                )
        
    @classmethod
    def register(cls, *args, **kwargs):
        """Called in app.py to register endpoints"""

        # Socket integration
        socket = kwargs.get('socket', None)
        if socket and hasattr(cls, cls.socket_cache_name):
            cls.add_socket_routes(socket)
        
        del kwargs['socket']
        
        try:
            super(FlaskView, cls).register(*args, **kwargs)
        except IgnoredFlaskClassyFunction:
            pass


def route(path="", **options):
    """Decorator for flask-classy, that allows you to specify options, without
    specifying the path."""
    def decorator(f):
        local_path = path   # needed for scope reasons
        if local_path == '':
            local_path = f.__name__
        
        return flask_classy.route(local_path, **options)(f)

    return decorator


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
