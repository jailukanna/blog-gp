---
title: socket example sound (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sound'


Modules used in program: 
* `import os`
* `import time`

## python sound

Python socket example: sound

```python
from kivy.app import App
from kivy.uix.button import Button
from kivy.clock import Clock
from kivy.core.audio import SoundLoader
import time
import os
from subprocess import Popen, PIPE

class TestApp(App):
    def __init__(self, **kwargs):
        super(TestApp, self).__init__(**kwargs)
        self.count = 0
        self.playing = False

    def build(self):
        self.sound = SoundLoader.load('examples/audio/12909_sweet_trip_mm_clap_lo.wav')
        return Button(text='Test', on_press=self.play)

    def get_file_count(self):
        p = Popen('ls -l /proc/%s/fd | wc -l' % os.getpid(), shell=True, stdout=PIPE, stderr=PIPE)
        out, err = p.communicate()
        if p.returncode == 0:
            return int(out.rstrip())

    def play(self, *_):
        if not self.playing:
            self.playing = True
            self.count = self.get_file_count()
            print('starting count: %d' % self.count)
            for i in range(10):
                Clock.schedule_once(self._play, i)
            Clock.schedule_once(self.finish, 11)

    def _play(self, *_):
        print('playing sound')
        self.sound.play()
        self.print_count()

    def finish(self, *_):
        print('finishing')
        self.print_count()
        self.playing = False

    def print_count(self, *_):
        currcount = self.get_file_count()
        print('start: %d  current: %d  change: %d' % (self.count, currcount, currcount - self.count))

if __name__ == '__main__':
    TestApp().run()

'''
kivy/master:

starting count: 17
playing sound
start: 17  current: 18  change: 1
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 27  change: 10
playing sound
start: 17  current: 29  change: 12
playing sound
start: 17  current: 31  change: 14
playing sound
start: 17  current: 33  change: 16
playing sound
start: 17  current: 35  change: 18
playing sound
start: 17  current: 37  change: 20
playing sound
start: 17  current: 39  change: 22
playing sound
start: 17  current: 41  change: 24
finishing
start: 17  current: 37  change: 20
starting count: 37
playing sound
start: 37  current: 43  change: 6
playing sound
start: 37  current: 45  change: 8
playing sound
start: 37  current: 47  change: 10
playing sound
start: 37  current: 49  change: 12
playing sound
start: 37  current: 51  change: 14
playing sound
start: 37  current: 53  change: 16
playing sound
start: 37  current: 55  change: 18
playing sound
start: 37  current: 57  change: 20
playing sound
start: 37  current: 59  change: 22
playing sound
start: 37  current: 61  change: 24
finishing
start: 37  current: 57  change: 20


----------------------------------

kived/gstplayer-socket:

starting count: 17
playing sound
start: 17  current: 18  change: 1
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
playing sound
start: 17  current: 25  change: 8
finishing
start: 17  current: 19  change: 2
starting count: 19
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
playing sound
start: 19  current: 25  change: 6
finishing
start: 19  current: 19  change: 0
'''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
