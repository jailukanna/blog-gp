---
title: pil example moviepy time accuracy (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'moviepy time accuracy'

Functions in program: 
* `def makeframe(t):`

Modules used in program: 
* `import moviepy.editor as mpy`
* `import PIL.Image as plim`

## python moviepy time accuracy

Python pil example: moviepy time accuracy

```python
"""
This script checks the time accuracy of MoviePy.

First, a one-hour video is generated, where the frame
at time t displays t (in seconds, e.g. '1200.50') in white
on a black baground.

Then we ask MoviePy to open this video file, fetch
different times (1200.5, 850.2, 2000.3, 150.25, 150.25),
extract the corresponding frame as a JPEG image file, and
check that the time indicated in the frame corresponds to
the time requested (up to 0.04 seconds, because the video
is 25 fps).

Status:
Works perfectly with these formats: .mp4, .ogv, .webm.
Other formats not tested but should work perfectly too.
"""

import PIL.Image as plim
from PIL import ImageFont, ImageDraw
import moviepy.editor as mpy
from moviepy.video.io.bindings import PIL_to_npimage



# For speed we will use PIL/Pillow to draw the texts
# instead of the simpler/slower TextClip() option

fontname = "/usr/share/fonts/truetype/freefont/FreeMono.ttf"
font = ImageFont.FreeTypeFont(fontname, 24)

def makeframe(t):
    im = plim.new('RGB',(200,100))
    draw = ImageDraw.Draw(im)
    draw.text((50, 25), "%.02f"%(t), font=font)
    return PIL_to_npimage(im)

clip = mpy.VideoClip(makeframe, duration=3600)



# Write the 1h-long clip to a file (takes 2 minutes)
# You can change the extension to test other formats

one_hour_filename = "one_hour.mp4" # Or .ogv, .webm
clip.write_videofile(one_hour_filename, fps=25)



# We now read the file produced and extract frames at several
# times. Check that the frame content matches the time.

new_clip = mpy.VideoFileClip(one_hour_filename)
for t in [0,1200.5, 850.2, 2000.3, 150.25, 150.25]:
    new_clip.save_frame('%d.jpeg'%(int(100*t)),t)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
