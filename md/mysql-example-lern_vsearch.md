---
title: mysql example lern vsearch (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'lern vsearch'

Functions in program: 
* `def search4letters(phrase: str, letters: str='aeiou') -> set:`
* `def search4vowels(phrase: str) -> set:`

## python lern vsearch

Python mysql example: lern vsearch

```python
def search4vowels(phrase: str) -> set:
    """Returns thr set of vowels found in 'phrase'."""
    return set('aeiou').intersection(set(phrase))

def search4letters(phrase: str, letters: str='aeiou') -> set:
    """Returns thr set of 'letters' found in 'phrase'."""
    return set(letters).intersection(set(phrase))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
