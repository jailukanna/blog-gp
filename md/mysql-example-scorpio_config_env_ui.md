---
title: mysql example scorpio config env ui (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio config env ui'

Functions in program: 
* `def get_current_env(test_module: TestModuleEnum):`

## python scorpio config env ui

Python mysql example: scorpio config env ui

```python
# coding: utf-8
from enum import Enum

__QA_55 = "http://192.168.130.55"
__QA_83 = "http://192.168.130.83"
__QA_IDC = "http://192.168.200.22"
__RD_102 = "http://192.168.130.102"
__RD_103 = "http://192.168.130.103"

# ==================================================
# 当前测试Server环境
# ==================================================
current_root = __QA_55


# ==================================================
# 测试模块枚举
# ==================================================
class TestModuleEnum(Enum):
    PAS = ":5003/projects/"
    CONSOLE_RELATIONS = ":5002/relations/"
    BATCHLOAD = ":5004/batchLoad"


def get_current_env(test_module: TestModuleEnum):
    return current_root + test_module.value


if __name__ == "__main__":
    print(get_current_env(TestModuleEnum.PAS))
    print(get_current_env(TestModuleEnum.CONSOLE_RELATIONS))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
