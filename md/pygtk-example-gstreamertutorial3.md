---
title: pygtk example gstreamertutorial3 (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'gstreamertutorial3'


Modules used in program: 
* `import gtk.glade`
* `import gtk`
* `import pygtk`
* `import gst`
* `import pygst`

## python gstreamertutorial3

Python pygtk example: gstreamertutorial3

```python
#!/usr/bin/python
import pygst
pygst.require("0.10")
import gst
import pygtk
import gtk
import gtk.glade

'''
gst-launch -e videomixer name=mix \
sink_0::xpos=0 sink_0::ypos=0 sink_0::alpha=0 \
sink_1::xpos=100 sink_1::ypos=50 \
! xvimagesink \
videotestsrc \
! video/x-raw-yuv,width=600,height=200 \
! mix.sink_0 \
videotestsrc pattern=0 \
! video/x-raw-yuv,width=100,height=100 \
! mix.sink_1 
'''

class Main:
	def __init__(self):

		# Create gui bits and bobs

		self.wTree = gtk.glade.XML("gui.glade", "mainwindow")
		
		signals = {
			"on_play_clicked" : self.OnPlay,
			"on_stop_clicked" : self.OnStop,
			"on_quit_clicked" : self.OnQuit,
		}

		self.wTree.signal_autoconnect(signals)

		# Create GStreamer bits and bobs

		self.pipeline = gst.Pipeline("mypipeline")

		#self.videotestsrc = gst.element_factory_make("videotestsrc", "videoin")
		
		source = gst.element_factory_make("videotestsrc", "video-source-bg")
		source1 = gst.element_factory_make("videotestsrc", "video-source1")
		source1.set_property("pattern",0)
		source2 = gst.element_factory_make("videotestsrc", "video-source2")
		source2.set_property("pattern",4)
		
		sink = gst.element_factory_make("xvimagesink", "video-output")
		caps_bg = gst.Caps("video/x-raw-yuv, width=600, height=200")
		filter = gst.element_factory_make("capsfilter", "filter-bg")
		filter.set_property("caps", caps_bg)
		filter_src_pad=filter.get_pad("src")
		
		caps1 = gst.Caps("video/x-raw-yuv, width=100, height=100")
		filter1 = gst.element_factory_make("capsfilter", "filter1")
		filter1.set_property("caps", caps1)
		filter1_src_pad=filter1.get_pad("src")
		
		caps2 = gst.Caps("video/x-raw-yuv, width=100, height=100")
		filter2 = gst.element_factory_make("capsfilter", "filter2")
		filter2.set_property("caps", caps2)
		filter2_src_pad=filter2.get_pad("src")
		
		mixer = gst.element_factory_make("videomixer", "mixer")
		#mixer.set_property("background",3)
		
		#self.bin = gst.Bin("my-bin")
		pad0 = mixer.get_pad("sink_0")
		#pad0 = gst.Element.get_pad("sink_0")
		pad1 = mixer.get_pad("sink_1")
		pad2 = mixer.get_pad("sink_2")
		
		#sink_0
		pad0.set_property("xpos",0)
		pad0.set_property("ypos",0)
		pad0.set_property("alpha",0)
		
		#sink_1
		pad1.set_property("xpos",0)
		pad1.set_property("ypos",0)

		#sink_2
		pad2.set_property("xpos",200)
		pad2.set_property("ypos",0)
		
		#get src pad of stream1, attach it to sink_%d of videomixer
		#we add all elements into the pipeline
		self.pipeline.add(source,filter,mixer,sink)
		self.pipeline.add(source1,filter1)
		self.pipeline.add(source2,filter2)

		#we link the elements together
		gst.element_link_many(source,filter)
		gst.element_link_many(source1,filter1)
		gst.element_link_many(source2,filter2)
		#link pad manually
		filter_src_pad.link(pad0)
		#filter.link_pads(filter_src_pad,mixer,pad0)
		filter1_src_pad.link(pad1)
		filter2_src_pad.link(pad2)
		
		#mixer.link(sink)
		gst.element_link_many(mixer,sink)
		
		#---BG
		#self.pipeline.add(source, source1, filter, filter1, mix, sink)
		#link srouce 1
		#gst.element_link_many(source, filter, mix, sink)
		#link source 2 
		#gst.element_link_many(source2, filter, mix, sink)


		#create multiple pads for the mixer element.
		#Aggregator pad, several inputs into one source.
		#pipeline has already been created 	
		#m=self.pipeline.get_by_name("mixer")

		
				
		
		
		
		

		self.window = self.wTree.get_widget("mainwindow")
		self.window.show_all()
		
		#bus = self.pipeline.get_bus()
		#bus.add_signal_watch()
		#bus.enable_sync_message_emission()
		#bus.connect("message", self.on_message)

	def OnPlay(self, widget):
		print("play")
		self.pipeline.set_state(gst.STATE_PLAYING)

	def OnStop(self, widget):
		print("stop")
		self.pipeline.set_state(gst.STATE_READY)
		
	def OnQuit(self, widget):
		gtk.main_quit()
		
	def on_message(self, bus, message):
		t = message.type
		if t == gst.MESSAGE_EOS:
			self.pipeline.set_state(gst.STATE_NULL)
			self.button.set_label("Start")
		elif t == gst.MESSAGE_ERROR:
			err, debug = message.parse_error()
			print("Error: %s" % err, debug)
			self.pipeline.set_state(gst.STATE_NULL)
			self.button.set_label("Start")		

start=Main()
gtk.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
