---
title: pygtk example snapshot (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'snapshot'

Functions in program: 
* `def get_filename(filename, itr=None, ext='.png'):`
* `def snapshot(source_dir, destination_dir, num_snaps, max_width=100):`

Modules used in program: 
* `import pdb`
* `import datetime`
* `import sys`

## python snapshot

Python pygtk example: snapshot

```python
#!/usr/bin/env python

#import pygst
#pygst.require('0.10')
from gi.repository import Gtk
Gtk.init(None)

from gi.repository import Gst, GstVideo
Gst.init(None)
#import pygtk
#import gtk
#from PIL import Image

#from repoze.filesafe import create_file
#import scaler
from perfect_thumb import PerfectThumb

import sys
import datetime

import pdb


def snapshot(source_dir, destination_dir, num_snaps, max_width=100):
#    ## Debugger ###############################################
    Gst.debug_set_default_threshold(Gst.DebugLevel.LOG)
    Gst.debug_set_threshold_for_name('gdkpixbufsink', Gst.DebugLevel.LOG)
#    ##########################################################

##   ## OLD #######
#    CAPS = "video/x-raw-rgb,pixel-aspect-ratio=1/1,bpp=(int)24,depth=(int)24,endianness=(int)4321,red_mask=(int)0xff0000, green_mask=(int)0x00ff00, blue_mask=(int)0x0000ff"
#    descr = 'uridecodebin uri=%s ! ffmpegcolorspace ! videoscale ! appsink name=sink caps="%s"' % (source_path, CAPS)
#    CAPS = "video/x-raw,format=RGB,pixel-aspect-ratio=1/1"

    #####################################################################
    ## description of pipeline
    descr = 'uridecodebin uri=%s ! videoconvert ! videoscale ! gdkpixbufsink name=sink' % (source_dir)

    ## pipeline created
    pipeline = Gst.parse_launch(descr)

    ## getting part of pipeline
    sink = pipeline.get_by_name('sink')

    ##############################################################
    ## set to PAUSED to make the first frame arrive in the sink
    ret = pipeline.set_state(Gst.State.PAUSED)
    print('ret: ', ret)
    ## error check
    if ret == Gst.StateChangeReturn.FAILURE:
        print('failed to play the file (1)\n')
    elif ret == Gst.StateChangeReturn.NO_PREROLL:
        print('live sources not supported yet\n')

    ############################################
    ##  getting the element's state (protected with a timeout)
    ret = pipeline.get_state(2 * Gst.SECOND)

    ##  error check
    if ret[0] == Gst.StateChangeReturn.FAILURE:
        print('failed to play the file (2)\n')
        sys.exit()

    #############################################
    ## getting the duration
    format = Gst.Format.TIME ## choices besides TIME to put thru query_duration?
    duration = pipeline.query_duration(format)[1]
    position = duration * 0.5
    pdb.set_trace()
    print('KEY_UNIT: ', Gst.SeekFlags.KEY_UNIT)
    print('FLUSH: ', Gst.SeekFlags.FLUSH)
    ret = pipeline.seek_simple(Gst.Format.TIME,
                               Gst.SeekFlags.KEY_UNIT | Gst.SeekFlags.FLUSH,
                               position)
    ## giving it time to work
    ret = pipeline.get_state(5 * Gst.SECOND)

    print('ret: ', ret)
    ## error check
    if not ret:
        print('failed to seek forward\n')
        sys.exit()
    if ret == Gst.StateChangeReturn.FAILURE:
        print('failed to play the file (1)\n')
    elif ret == Gst.StateChangeReturn.NO_PREROLL:
        print('live sources not supported yet\n')


    ## NEWLY ADDED BLOCK ##########################################
    ##  getting the element's state (protected with a timeout)
    ret = pipeline.get_state(5 * Gst.SECOND)
    ##  error check
    if ret[0] == Gst.StateChangeReturn.FAILURE:
        print('failed to seek forward\n')
        sys.exit()

    pixbuf = sink.props.last_pixbuf
    h = pixbuf.get_height()
    w = pixbuf.get_width()
    rowstride = w*3 + (w*3 % 4)

    ##############################################
    ##
    for snap in xrange(num_snaps):

#        if duration > 0:
#            position = duration * (snap * 0.05) ## to go 5% into the video
#        else:
#            position = 1 * Gst.SECOND ## seek to 1 second (this could cause EOS)

#        pipeline.seek_simple(Gst.Format.TIME,
#                             Gst.SeekFlags.KEY_UNIT | Gst.SeekFlags.FLUSH,
#                             position)



        ################################
        ## choosing a desired path/to/filename ####
        filename = 'snapshot-' + str(snap) + '.png'
        dest_dir = destination_dir
        dest_dir = dest_dir if dest_dir[-1] == '/' else dest_dir + '/'
        destination_path = dest_dir + filename

        ## shrink it and save it
        PerfectThumb.shrink_raw(pixbuf.get_pixels(),
                                destination_path,
                                w, h, rowstride,
                                xy=max_width)

#            pixbuf.save('snapshot.png', 'png')


    pipeline.set_state(Gst.State.NULL)

    sys.exit()

def get_filename(filename, itr=None, ext='.png'):
    return filename + ext


thumb_path = '/home/raj/vbox_shared'
source_path = 'file:///home/raj/vbox_shared/rajbday.mpg'

start = snapshot(source_path, thumb_path, 3)
#gtk.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
