---
title: socket example socketread (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketread'

Functions in program: 
* `def printtheframe(frame):`

Modules used in program: 
* `import aprs`
* `import kiss`

## python socketread

Python socket example: socketread

```python
import kiss
import aprs

k = kiss.KISS(host='10.10.0.106', port=8001)
k.logger.LOG_LEVEL = "INFO"
k.start()  # inits the TNC, optionally passes KISS config flags.

def printtheframe(frame):

    frame_len = len(frame)
    if frame_len > 16:
        frame = frame.lstrip('\x00')    # This is the line that made it start working!!!
        frame = frame.strip()
        try:
            # Decode raw APRS frame into dictionary of separate sections
            decoded_frame = aprs.util.decode_frame(frame)
            # Format the APRS frame (in Raw ASCII Text) as a human readable frame
            formatted_aprs = aprs.util.format_aprs_frame(decoded_frame)  # frame['destination'] must be removed from this function
            print(formatted_aprs           # This is the human readable APRS output - IT WORKS!)

        except:
            print("Error decoding frame:")
            print("    " + frame)

k.read(callback=printtheframe)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
