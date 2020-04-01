---
title: socket example requests (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'requests'

Functions in program: 
* `def set_request_retries(retries):`
* `def set_request_timeout(timeout):`
* `def _enable_retries(max_retries):`

Modules used in program: 
* `import requests`

## python requests

Python socket example: requests

```python
"""
Monkey-Patch Requests for automatic timeouts and retries

Retries for rrDNS, timeouts because you should always do timeouts.
If you want these features, just do ```from $MYPACKAGE.requests import requests```.

"""""

import requests

DEFAULT_REQUEST_TIMEOUT = 5
DEFAULT_REQUEST_RETRIES = 2


def _enable_retries(max_retries):
    """
    Decorator for `requests.adapters.HTTPAdapter.__init__`.

    Enables retries in the requests module.
    Due to behavior I found when trying to simply set the default
    argument for retries in the HTTPAdapter.__init__ method (hitting
    max recursion depth), we simply wrap the __init__ method &
    manually set the Retry object on the HTTPAdapter class.
    """

    def outer(adapter_init):
        retry_obj = requests.packages.urllib3.util.retry.Retry(total=max_retries, backoff_factor=0.1)

        def inner(self, *args, **kwargs):
            adapter_init(self, *args, **kwargs)
            # Self is the adapter instance passed in above
            self.max_retries = retry_obj

        return inner

    return outer


def set_request_timeout(timeout):
    """Set global socket timeout & requests request timeout"""
    # Set default timeout (global and in request package)
    import socket  # keep reference local
    from functools import partialmethod

    socket.setdefaulttimeout(timeout)
    requests.sessions.Session.request = partialmethod(requests.sessions.Session.request, timeout=timeout)


def set_request_retries(retries):
    """Set max num retries for requests (the package)"""
    # Get func with reference to max_retries `Retry` object
    enabler = _enable_retries(retries)

    # Wrap __init__ method of HTTPAdapter
    new_init = enabler(requests.adapters.HTTPAdapter.__init__)

    # Apply wrapper __init__ back to HTTPAdapter
    requests.adapters.HTTPAdapter.__init__ = new_init

# I like to automatically set these on import so I don't forget
# If you want more control just don't include these and/or set
# them manually when your code starts
set_request_timeout(DEFAULT_REQUEST_TIMEOUT)
set_request_retries(DEFAULT_REQUEST_RETRIES)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
