---
title: pil example subs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'subs'

Functions in program: 
* `def process(pos, output, img):`
* `def levenshtein(s1, s2):`

Modules used in program: 
* `import sys`
* `import re`
* `import multiprocessing as mp`
* `import numpy as np`
* `import unicodedata`
* `import time`
* `import srt #https://media.readthedocs.org/pdf/srt/latest/srt.pdf`
* `import cv2`
* `import pyocr`

## python subs

Python pil example: subs

```python
import pyocr
from PIL import Image
import cv2
import srt #https://media.readthedocs.org/pdf/srt/latest/srt.pdf
from datetime import timedelta
import time
import unicodedata
import numpy as np
import multiprocessing as mp
import re
import sys

# file settings
FILE=sys.argv[1]
FILE_OUT=sys.argv[2]
print(FILE, FILE_OUT)
LANG = ''
TEXT_TOP = 560
TEXT_BOTTOM = 720
TEXT_LEFT = 0
TEXT_RIGHT = 1280
FR_SKIP = 3
FR_DELTA = timedelta(seconds=1001/24000)*(FR_SKIP+1) # 24fps
NTHREADS = 13
# comment below to not skip
SKIP_TO = timedelta(minutes=0,seconds=0)
# misc image processing options
START_REGION, END_REGION = (245,245,245), (255, 255, 255)
KERNEL = np.ones((3,3),np.uint8)
LEV_LIM = 7
SYMBOL_LIMS = [' ', '.', '¿','?','-','!', ',']
WORD_FILTER = re.compile(r"^[ABCDFGHJKLMNPQRSTVXZabcdfghjklmnpqrstvxz]+$")
ABSDIFF_THRESH=0.5
EMPTY_THRESH=0.1

# runtime
n_time = timedelta()
subs = []
tool = pyocr.get_available_tools()[0]
cap = cv2.VideoCapture(FILE)
last = 0
last_img = None

if "SKIP_TO" in vars():
    n_time += SKIP_TO
    cap.set(cv2.CAP_PROP_POS_MSEC, SKIP_TO.total_seconds()*1000)

def levenshtein(s1, s2):
    if len(s1) > len(s2):
        s1, s2 = s2, s1
    dst = range(len(s1) + 1)
    for i2, c2 in enumerate(s2):
        dst_ = [i2+1]
        for i1, c1 in enumerate(s1):
            if c1 == c2:
                dst_.append(dst[i1])
            else:
                dst_.append(1 + min((dst[i1], dst[i1 + 1], dst_[-1])))
        dst = dst_
    return dst[-1]

def process(pos, output, img):
    img = cv2.morphologyEx(img, cv2.MORPH_CLOSE, KERNEL)
    pil_img = Image.fromarray(img)
    #pil_img.show()

    # process text
    extracted = tool.image_to_string(pil_img, lang=LANG)
    extracted = extracted.replace("\n", " ")
    text = []
    for c in extracted:
        cat = unicodedata.category(c)
        if cat == 'Ll' or cat == 'Lu' or c in SYMBOL_LIMS:
            text.append(c)
    text = ''.join(text).strip()
    # delete one length words
    words = text.split(' ')
    words = list(filter(lambda word: not (len(word)<=1 or WORD_FILTER.search(word)!=None), words))
    text = ' '.join(words)
    output.put((pos, text))

try:
    output = mp.Queue()
    while(cap.isOpened()):
        processes = []
        for x in range(NTHREADS):
            _, img = cap.read()
            for i in range(FR_SKIP): cap.grab()
            img = img[TEXT_TOP:TEXT_BOTTOM, TEXT_LEFT:TEXT_RIGHT]
            img = cv2.inRange(img, START_REGION, END_REGION)
            if np.average(img) <= EMPTY_THRESH: # skip empty frames
                output.put((x, None))
            elif last_img is not None:
                diff = np.average(cv2.absdiff(img, last_img))
                if diff > ABSDIFF_THRESH:
                    processes.append(mp.Process(target=process, args=(x, output, img)))
                else: # don't process similar frames
                    output.put((x, -1))
            else:
                processes.append(mp.Process(target=process, args=(x, output, img)))
            last_img = img

        # run processes
        for p in processes:
            p.start()
        # Exit the completed processes
        for p in processes:
            p.join()

        # get + sort
        results = [output.get() for i in range(NTHREADS)]
        results.sort()
        results = [r[1] for r in results]

        for i, result in enumerate(results):
            if result == -1:
                if i == 0:
                    if subs: results[i] = subs[-1].content
                    else: results[i] = ''
                else: results[i] = results[i-1]

        # add to sub
        for text in results:
            if text:
                if subs and levenshtein(subs[-1].content,text) <= LEV_LIM:
                    subs[-1].end += FR_DELTA
                else:
                    subs.append(srt.Subtitle(index=len(subs)+1,
                                            start=n_time,
                                            end=(n_time+FR_DELTA),
                                            content=text))
            n_time += FR_DELTA

        print(time.time()-last,n_time, results)
        last = time.time()
except:
    pass

cap.release()

print(subs)
print("Finished composing, writing to file...")
with open(FILE_OUT, "w") as f:
    f.write(srt.compose(subs))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
