---
title: matplotlib example mac%25EC%2597%2590%25EC%2584%259C%2520matplotlibplot%2520%25EC%2597%2590%25EB%259F%25AC%25EB%25B0%259C%25EC%2583%259D (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mac%25EC%2597%2590%25EC%2584%259C%2520matplotlibplot%2520%25EC%2597%2590%25EB%259F%25AC%25EB%25B0%259C%25EC%2583%259D'


Modules used in program: 
* `import matplotlib`

## python mac%25EC%2597%2590%25EC%2584%259C%2520matplotlibplot%2520%25EC%2597%2590%25EB%259F%25AC%25EB%25B0%259C%25EC%2583%259D

Python matplotlib example: mac%25EC%2597%2590%25EC%2584%259C%2520matplotlibplot%2520%25EC%2597%2590%25EB%259F%25AC%25EB%25B0%259C%25EC%2583%259D

```python
## matplotlib.pyplot 사용시 에러발생
># 문제발생
에러문구는 아래와 같다
```
backend_mod = importlib.import_module(backend_name)
  File "/Users/test/.pyenv/versions/3.5.6/lib/python3.5/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "/Users/test/.pyenv/versions/3.5.6/lib/python3.5/site-packages/matplotlib/backends/backend_macosx.py", line 14, in <module>
    from matplotlib.backends import _macosx
ImportError: Python is not installed as a framework. 
```
># 문제원인
```
print((matplotlib.rcsetup.all_backends))
['GTK3Agg', 'GTK3Cairo', 'MacOSX', 'nbAgg', 'Qt4Agg', 'Qt4Cairo', 'Qt5Agg', 'Qt5Cairo', 
'TkAgg', 'TkCairo', 'WebAgg', 'WX', 'WXAgg', 'WXCairo', 'agg', 'cairo', 'pdf', 'pgf', 'ps', 'svg', 'template']
```
matplotlib는 다양한 케이스에서 동작할 수 있도록 여러가지 backend - user interfce를 가지고 있는데, 현재 사용하는 os와 맞지 않아
충돌이 나서 생기는 문제이므로 어떤 backend를 사용할 것인지를 지정 해주면 된다.

># 문제해결
방법은 두가지가 존재한다
1. ~/.matplotlib 에서 backend 설정을 바꾸기
pip install 로 설치했을 경우에는, root directory에 ~/.matplotlib가 생기는데 이 디렉토리에 matplotlibrc 설정파일을 만들어서 지정해준다.
backend:TKAgg # 사용하고 싶은 backend 입력

2. Python script 코드에서 설정 바꾸기
matplotlib에서 use() function을 사용하는 방법이다

import matplotlib
matplotlib.use('TKAgg')





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
