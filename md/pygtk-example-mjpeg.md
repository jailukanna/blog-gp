---
title: pygtk example mjpeg (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'mjpeg'


Modules used in program: 
* `import threading`
* `import gobject`
* `import urllib`
* `import gtk`
* `import pygtk`

## python mjpeg

Python pygtk example: mjpeg

```python
import pygtk
pygtk.require('2.0')
import gtk
import urllib
import gobject
import threading

STREAM_URL = 'http://10.10.1.1:8196/'

class VideoThread(threading.Thread):
    '''
    A background thread that takes the MJPEG stream and
    updates the GTK image.
    '''
    def __init__(self, widget):
        super(VideoThread, self).__init__()
        self.widget = widget
        self.quit = False
        print('connecting to', STREAM_URL)
        self.stream = urllib.urlopen(STREAM_URL)

    def get_raw_frame(self):
        '''
        Parse an MJPEG http stream and yield each frame.
        Source: http://stackoverflow.com/a/21844162

        :return: generator of JPEG images
        '''
        raw_buffer = ''
        while True:
            new = self.stream.read(1034)
            if not new:
                # Connection dropped
                yield None
            raw_buffer += new
            a = raw_buffer.find('\xff\xd8')
            b = raw_buffer.find('\xff\xd9')
            if a != -1 and b != -1:
                frame = raw_buffer[a:b+2]
                raw_buffer = raw_buffer[b+2:]
                yield frame

    def run(self):
        for frame in self.get_raw_frame():
            if self.quit or frame is None:
                return
            loader = gtk.gdk.PixbufLoader('jpeg')
            loader.write(frame)
            loader.close()
            pixbuf = loader.get_pixbuf()
            # Schedule image update to happen in main thread
            gobject.idle_add(self.widget.set_from_pixbuf, pixbuf)

if __name__ == '__main__':
    gobject.threads_init()
    window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    window.connect("destroy", gtk.main_quit)
    window.set_border_width(10)

    img = gtk.Image()
    img.show()
    window.add(img)
    t = VideoThread(img)
    window.show()

    t.start()
    gtk.main()
    t.quit = True


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
