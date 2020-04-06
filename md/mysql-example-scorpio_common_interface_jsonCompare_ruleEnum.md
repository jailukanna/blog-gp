---
title: mysql example scorpio common interface jsonCompare ruleEnum (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common interface jsonCompare ruleEnum'


## python scorpio common interface jsonCompare ruleEnum

Python mysql example: scorpio common interface jsonCompare ruleEnum

```python
from enum import Enum


class Rule(Enum):
    IS_ANY_INTEGER = "${IS_ANY_INTEGER}"
    IS_ANY_FLOAT = "${IS_ANY_FLOAT}"
    IS_ANY_STRING = "${IS_ANY_STRING}"
    IS_ANY_BOOL = "${IS_ANY_BOOL}"
    IS_TIMESTEMP = "${IS_TIMESTEMP}"  # '%Y-%m-%d %H:%M:%S'

    MATCH_REGEX = "${MATCH_REGEX}"

    IGNORE_VALUE = "${IGNORE_VALUE}"
    IGNORE_OBJECT_KEY_MISS_MATCH = "${IGNORE_OBJECT_KEY_MISS_MATCH}"
    IGNORE_ARRAY_SIZE = "${IGNORE_ARRAY_SIZE}"

    IS_JSON_OBJECT = "${IS_JSON_OBJECT}"
    IS_JSON_ARRAY = "${IS_JSON_ARRAY}"
    IS_JSON_PRIMITIVE = "${IS_JSON_PRIMITIVE}"


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
