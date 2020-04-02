---
title: PIL example pil (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pil'

Functions in program: 
* `def ensure_PIL():`

## python pil

Python PIL example: pil

```python
def ensure_PIL():
    try:
        import PIL

    except ModuleNotFoundError:
        print("Installing PIL")
        
        import subprocess
        r = subprocess.run(["pip", "install", "Pillow"])

        if r.returncode == 0:
            import PIL
            
        else:
            print("PIL could not be installed")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
