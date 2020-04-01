---
title: socket example api cmds-base (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'api cmds-base'


## python api cmds-base

Python socket example: api cmds-base

```python
from cmds.base import FlaskView as CmdFlaskView


class FlaskView(CmdFlaskView):
    route_prefix = '/api'

    @classmethod
    def on_socket_event(cls, event_name):
        """Registers a listener for websocket events.
        
        We cache all socket listeners in `_socket_rule_cache` within the parent class.
        Then, on register, we iterate through these listeners and assign them to the socket.

        NOTE: We need to make this part of the class, so it's aware of itself.
        """
        def decorator(f):
            # Format: 'function_name': [<function>, <tagA>, <tagB>, ...]
            # NOTE: This is how you layout a decorator that takes in input.

            if not hasattr(cls, cls.socket_cache_name) or \
                    getattr(cls, cls.socket_cache_name) is None:
                # Initialize cache
                setattr(cls, cls.socket_cache_name, {
                    f.__name__: [f, event_name]
                })
            else:
                cache = getattr(cls, cls.socket_cache_name)

                if f.__name__ in cache:
                    cache[f.__name__].append(event_name)
                else:
                    cache[f.__name__] = [f, event_name]

            def returned_func(*args, **kwargs):
                return f(*args, **kwargs)

            return returned_func

        return decorator


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
