---
title: pil example pyramid (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pyramid'


Modules used in program: 
* `import os`

## python pyramid

Python pil example: pyramid

```python
from math import sqrt
import os

from video_file_clip import VideoFileClip
from moviepy.video.VideoClip import ColorClip
from moviepy.video.compositing.CompositeVideoClip import CompositeVideoClip

class Pyramid:
    def __init__(self, display_width, display_height, clip_file):
        self.clip = VideoFileClip(clip_file)

        self.W = display_width
        self.H = display_height

        self.side = int(display_width/2)
        self.main = int(sqrt((display_width/2)**2 + display_height**2))

    def create_videohologram(self, output_file, side_offset=20, main_offset=0):
        background_clip = ColorClip((self.W, self.H), (0, 0, 0), duration=0)
        resized_clip = self.resize_clip(self.clip)

        left_clip = resized_clip.rotate(-90)
        x = int((self.W * left_clip.size[1]) / (2 * self.H + side_offset))

        left_pos = (self.W / 2 - x - left_clip.size[0], side_offset)
        left_clip = left_clip.set_position(left_pos)

        main_pos = ((self.W / 2) - resized_clip.size[0] / 2, resized_clip.size[0] + main_offset)
        main_clip = resized_clip.set_position(main_pos)

        right_clip = resized_clip.rotate(90)
        right_pos = (self.W / 2 + x, side_offset)
        right_clip = right_clip.set_position(right_pos)

        video = CompositeVideoClip([background_clip, main_clip, left_clip, right_clip])
        video.write_videofile(output_file)
        os.startfile(output_file)

    def resize_clip(self, clip):
        width, height = clip.size

        if width >= height:
            return clip.resize(width=int(self.W/4))
        else:
            return clip.resize(height=int(self.W/4))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
