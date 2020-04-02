---
title: pil example consolidator (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'consolidator'

Functions in program: 
* `def main():`
* `def check_artwork(path, pattern):`
* `def check_type(filename):`
* `def check_mode(path):`

Modules used in program: 
* `import subprocess`
* `import re`
* `import os.path`
* `import os`

## python consolidator

Python pil example: consolidator

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


"""Music library consolidator.

1. Find file not 644 chmod to 644
2. Report file type not .m4a .pdf
3. Report ffprobe artwork if not 640x640 PNG
"""


import os
import os.path
import re
import subprocess


def check_mode(path):
    """ """
    mode = oct(os.stat(path).st_mode)[4:]
    return mode == "644"


def check_type(filename):
    """ """
    suffix = filename.split('.')[-1]
    return suffix == "m4a"


def check_artwork(path, pattern):
    """ """
    out = subprocess.check_output(["ffprobe", path], stderr=subprocess.STDOUT)
    for line in out.splitlines():
        if pattern.match(line):
            return True
    return False


def main():
    """ """
    ptn = re.compile(r"\s*Stream #0:0: Video: png, .*640x640, .*")

    for root, dirs, files in os.walk("/Users/mick/.tmp/rip/foo/jjlin"):
        if files:
            for filename in files:
                path = os.path.join(root, filename)

                if not check_mode(path):
                    print(">>> not 644: " + path)

                if not check_type(filename):
                    print(">>> not m4a: " + path)
                    pass
                else:
                    # We got a .m4a file.
                    if not check_artwork(path, ptn):
                        print(">>> bad artwork: " + path)


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
