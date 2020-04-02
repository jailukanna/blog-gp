---
title: PIL example openai join vids (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'openai join vids'


Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import subprocess`
* `import matplotlib`
* `import os`

## python openai join vids

Python PIL example: openai join vids

```python
%matplotlib inline
import os
import matplotlib
import subprocess
import numpy as np
from PIL import Image
import PIL

os.chdir('/tmp/tmpE2HLAF')

videos = os.listdir('.')
videos = filter(lambda x: 'mp4' in x, videos)
images= {}
for video in sorted(videos)[:25]:# Only decode 10 of the files
   if not os.path.exists(video[:-4]):
      os.makedirs(video[:-4])
      subprocess.call(['ffmpeg', '-i' , video ,  '-r', '50' , video[:-4] + '/' + video[:-4] + '%04d.png'])
   images[video[:-4]] = os.listdir(video[:-4])
   
for i in images.keys():
    images[i] = filter(lambda x: ".thumbnail.png" not in x, images[i])
# Make sure we don't get any thumbnails in the list of files
size = 128, 128
for i in images.keys():
    for f in images[i]:
        print(i + '/' + f)
        if not os.path.exists(i + "/" + f[:-4] + ".thumbnail.png"):
            img = Image.open(i + '/' + f)
            #img.resize((basewidth,hsize), PIL.Image.ANTIALIAS)
            img = img.resize((size), PIL.Image.ANTIALIAS)
            img.save(i + "/" + f[:-4] + ".thumbnail.png")
            
            
lengths = {}
the_max = 0
for i in sorted(images.keys()):
    lengths[i] = len(images[i]) 
    if lengths[i] > the_max:
        the_max = lengths[i]

for i in range(1,the_max+1):
   j = 0 
   q = 0
   out = np.array(Image.new( 'RGB', (128*5,128*5), "black"))
   for img in sorted(images.keys()):
        idx = i%lengths[img]
        if idx == 0:
            idx = 1
        print(idx, i)
        #openaigym.video.0.17897.video0000200087
        #openaigym.video.0.17897.video000020/openaigym.video.0.17897.video0000200087.thumbnail.png
        out[q*128:q*128+128, j%5*128:j%5*128+128,:] = np.array(Image.open(img + "/" + img + str(idx).zfill(4) + ".thumbnail.png"))
        j += 1
        q += int(j % 5 == 0)
   out = Image.fromarray(out)
   out.save('output/' + 'Picture' + str(i).zfill(4) + '.png')
 
# Ok for now you'll run this by hand at the end
#ffmpeg -r 50 -i output/Picture%04d.png -vcodec libx264 -crf 25 -pix_fmt yuv420p crash_and_burn.mp4

# Or with Sound
# ffmpeg -r 50 -i output/Picture%04d.png -i Robocop.mp3 -shortest -acodec copy -vcodec libx264 -crf 25 -pix_fmt yuv420p early_stages_audio.mp4


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
