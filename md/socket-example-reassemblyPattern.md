---
title: socket example reassemblyPattern (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'reassemblyPattern'


## python reassemblyPattern

Python socket example: reassemblyPattern

```python
fragments = []                # List of chunks
while not done:
     chunk = s.recv(maxsize)  # Get a chunk
     if not chunk:
         break                # EOF. No more data
     fragments.append(chunk)
# Reassemble the message
message = "".join(fragments)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
