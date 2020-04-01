---
title: socket example setup (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'setup'


Modules used in program: 
* `import sys`

## python setup

Python socket example: setup

```python
import sys
from setuptools import setup
from setuptools.command.test import test as TestCommand


# `python setup.py test` installs and runs `tox`
class Tox(TestCommand):
    def finalize_options(self):
        TestCommand.finalize_options(self)
        self.test_args = []
        self.test_suite = True

    def run_tests(self):
        #import here, cause outside the eggs aren't loaded
        import tox
        errno = tox.cmdline(self.test_args)
        sys.exit(errno)

setup(
    name='test-socket-ignore-signal',
    tests_require=['tox'],
    cmdclass={'test': Tox},
    url='https://gist.github.com/4069063',
    )


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
