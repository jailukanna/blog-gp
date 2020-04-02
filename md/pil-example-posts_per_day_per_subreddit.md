---
title: pil example posts per day per subreddit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'posts per day per subreddit'

Functions in program: 
* `def getRows(start, end):`

Modules used in program: 
* `import datetime`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sqlite3`

## python posts per day per subreddit

Python pil example: posts per day per subreddit

```python
from __future__ import division
import sqlite3
import numpy as np
from time import time
import matplotlib.pyplot as plt

def getRows(start, end):
    conn = sqlite3.connect(sqlitefile)
    c = conn.cursor()
    c.execute('''SELECT subreddit, created_utc FROM submissions WHERE rowid BETWEEN '''+str(start)+''' AND '''+str(end)+''';''')
    rows = c.fetchall()
    conn.close()
    return rows

# go through database and fill new NumPy array with posts per day per subreddit
data = np.zeros((430435,3507), dtype=np.uint16)
subreddits = {}
steps = int(196.5 / 0.5)
counter = 0
lasttime = time()
for s in range(0,steps):
    start = s*5e5+1
    stop = s*5e5+5e5
    rows = getRows(start,stop)
    for r in rows:
        # find subreddit index
        try:
            subreddit = subreddits[r[0]]
        except KeyError:
            subreddit = counter
            subreddits[r[0]] = subreddit
            counter += 1
        # find which day submissions was made
        day = int((r[1] - 1138057200) / 60/60/24 )
        # 1138057200 is the unix timestamp for January 24, 2006 00:00:00 (UTC),
        # so each bin will contain submissions from 00:00:00 to 23:59:59 for that day in UTC.
        
        data[subreddit,day] += 1
    print('Processed rows '+str(start)+' till '+str(stop)+' ('+str(stop/1965e5*100)+'%) ('+str(time() - lasttime)+' seconds) subreddit counter: '+str(counter))
    lasttime = time()

# make indexed list of subreddits
subreddits_int = np.empty(len(data)+1, dtype=object)
for key, value in subreddits.iteritems():
    subreddits_int[value] = key
    
# remove empty subreddits
inds = np.max(data, axis=1) > 0
data_nonempty = data[inds]
subreddits_int_nonempty = subreddits_int[inds]

# map to video pixels
# 1:1

offset = 26
field = np.zeros((658+offset,658))
# field = np.zeros((720,1280)) # 1:1 square in 16:9 frame
# spawn = [int(field.shape[0]/2), int(field.shape[1]/2)] # center
spawn = [int(field.shape[0]/2) + int(offset/2), int(field.shape[1]/2)] # center with offset

path = np.zeros((430435, 2), dtype=np.uint16)
path[0] = spawn
here = spawn
wantToGo = 'right'
for i in range(1,len(path)):
    try:
        if wantToGo == 'right':
            if field[here[0]+1, here[1]] == 0:
                here[0] += 1
                wantToGo = 'down'
            else:
                here[1] -= 1
        elif wantToGo == 'down':
            if field[here[0], here[1]+1] == 0:
                here[1] += 1
                wantToGo = 'left'
            else:
                here[0] += 1
        elif wantToGo == 'left':
            if field[here[0]-1, here[1]] == 0:
                here[0] -= 1
                wantToGo = 'up'
            else:
                here[1] += 1
        elif wantToGo == 'up':
            if field[here[0], here[1]-1] == 0:
                here[1] -= 1
                wantToGo = 'right'
            else:
                here[0] -= 1

        field[here[0], here[1]] = i
        path[i] = here
    except IndexError:
        rest = np.zeros((656,2))
        rest[:,1] = range(656)
        path[i:] = rest
        break
        
# get timestamp per frame
import datetime
dates = []
for i in range(data.shape[1]):
    d = datetime.datetime.fromtimestamp(1138057200+60*60*24*i)
    dates.append(d.strftime('%Y-%m-%d'))
    
# get total subreddit count per frame
subreddit_count = np.zeros(data.shape[1], dtype=int)
subreddit_count[0] = len(np.where(data[:,0])[0])
for i in range(1,data.shape[1]):
    x = data[subreddit_count[i-1]:,i]
    new_subs = np.where(x>0)[0]
    if len(new_subs) < 1:
        new_subs = 0
    else:
        new_subs = new_subs[-1]
    subreddit_count[i] = subreddit_count[i-1] + new_subs
    
# make and save frames
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

cmap = plt.get_cmap('inferno')
for i in range(629,data.shape[1]):
    frame = np.zeros((658+offset,658), dtype=np.uint8)
    
    for j,x in enumerate((data[:,i].clip(0, 10)*(255/10)).astype(np.uint8)):
        frame[path[j,0], path[j,1]] = x
        
    frame = np.delete(cmap(frame, bytes=True), 3, 2) # create rgba, delete alpha channel
    
    img = Image.fromarray(frame)
    draw = ImageDraw.Draw(img)
    # font = ImageFont.truetype(<font-file>, <font-size>)
    font = ImageFont.truetype("usr/share/fonts/truetype/freefont/FreeMono.ttf", 20)
    # draw.text((x, y),"Sample Text",(r,g,b))
    
    # add timestamp
    draw.text((6, 3), dates[i], (255,255,255),font=font)
    # add subreddit counter
    draw.text((450, 3), str(subreddit_count[i]).rjust(6)+' subreddits', (255,255,255),font=font)
    
    img.save('frames/'+str(i-629).zfill(4)+'.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
