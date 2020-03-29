---
title: pygtk example range-control (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'range-control'

Functions in program: 
* `def titlelabel(title):`
* `def packit(container,obj,arg1,arg2,arg3):`
* `def scale_set_default_value(scale):`
* `def make_menu_item(name,callback,data=None):`

Modules used in program: 
* `import gtk`
* `import pygtk`

## python range-control

Python pygtk example: range-control

```python
#!/usr/bin/python

import pygtk
pygtk.require('2.0')
import gtk

def make_menu_item(name,callback,data=None):
	item = gtk.MenuItem(name)
	item.connect('activate',callback,data)
	item.show()

	return item

def scale_set_default_value(scale):
	scale.set_update_policy(gtk.UPDATE_CONTINUOUS)
	scale.set_digits(1)
	scale.set_value_pos(gtk.POS_TOP)
	scale.set_draw_value(True)

def packit(container,obj,arg1,arg2,arg3):
	container.pack_start(obj,arg1,arg2,arg3)
	obj.show()

def titlelabel(title):
	box = gtk.HBox(False,10)
	box.set_border_width(10)

	label = gtk.Label('Scale Value Position:')
	packit(box,label,False,False,0)

	return box

class Range:
	def cb_pos_menu_select(self,item,pos):
		self.hscale.set_value_pos(pos)
		self.vscale.set_value_pos(pos)
	
	def cb_update_menu_select(self,item,policy):
		self.hscale.set_update_policy(policy)
		self.vscale.set_update_policy(policy)
	
	def cb_digits_scale(self,adj):
		self.hscale.set_digits(adj.value)
		self.vscale.set_digits(adj.value)
	
	def cb_page_size(self,get,set):
		set.page_size = get.value
		set.page_increment = get.value

		set.emit('changed')

	def cb_draw_value(self,button): #ok
		self.hscale.set_draw_value(button.get_active())
		self.vscale.set_draw_value(button.get_active())
	
	def __init__(self):
		#window
		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
		self.window.connect('destroy',lambda w:gtk.main_quit())
		self.window.set_title('range controls')

		#box1
		box1 = gtk.VBox(False,0)
		self.window.add(box1)
		box1.show()
		
		#scale and scollbar's container
		box2 = gtk.HBox(False,10)
		box2.set_border_width(10)
		packit(box1,box2,True,True,0)
		
		#adjustement
		adj1 = gtk.Adjustment(50.0,0.0,101.0,0.1,5.0,5.0)
		
		#Vscale
		self.vscale = gtk.VScale(adj1)
		scale_set_default_value(self.vscale)
		packit(box2,self.vscale,True,True,0)

		#Hscale
		box3 = gtk.VBox(False,10)
		packit(box2,box3,True,True,0)
		
		self.hscale = gtk.HScale(adj1)
		scale_set_default_value(self.hscale)
		packit(box3,self.hscale,True,True,0)
		
		#Hscrollbar
		scrollbar = gtk.HScrollbar(adj1)
		scrollbar.set_update_policy(gtk.UPDATE_CONTINUOUS)
		box3.pack_start(scrollbar,True,True,0)
		scrollbar.show()
		
		#display value
		box2 = gtk.HBox(False,10)
		box2.set_border_width(10)
		
		button = gtk.CheckButton("Display value on scale widgets")
		button.set_active(True)
		button.connect('toggled',self.cb_draw_value)
		packit(box2,button,True,True,0)
		packit(box1,box2,True,True,0)
		
		#value position
		box2 = titlelabel('Scale Value Position')
	
		opt = gtk.OptionMenu()
		menu = gtk.Menu()
		
		itemlist = (('Top',gtk.POS_TOP),('Bottom',gtk.POS_BOTTOM),
			    ('Left',gtk.POS_LEFT),('Right',gtk.POS_RIGHT))
		
		for itemobj in itemlist:
			item = make_menu_item(itemobj[0],self.cb_pos_menu_select,
					      itemobj[1])
			menu.append(item)
		
		opt.set_menu(menu)
		packit(box2,opt,True,True,0)
		packit(box1,box2,True,True,0)

		#update policy
		box2 = titlelabel('Scale Update Policy:')

		opt = gtk.OptionMenu()
		menu = gtk.Menu()

		itemlist = (('Continus',gtk.UPDATE_CONTINUOUS),
			    ('Discontinuous',gtk.UPDATE_DISCONTINUOUS),
			    ('Delayed',gtk.UPDATE_DELAYED))

		for itemobj in itemlist:
			item = make_menu_item(itemobj[0],self.cb_update_menu_select,
					      itemobj[1])
			menu.append(item)

		opt.set_menu(menu)

		packit(box2,opt,True,True,0)
		packit(box1,box2,True,True,0)
		
		#scale digits
		box2 = titlelabel('Scale Digits:')

		adj2 = gtk.Adjustment(1.0,0.0,5.0,1.0,1.0,1.0)
		adj2.connect('value_changed',self.cb_digits_scale)
		scale = gtk.HScale(adj2)
		scale.set_digits(0)
		packit(box2,scale,True,True,0)
		packit(box1,box2,True,True,0)
		
		#page size	
		#box2 = titlelabel('Scrollbar Page Size:')
		
		#adj2 = gtk.Adjustment(1.0,1.0,100.0,1.0,1.0,0.0)
		#adj2.connect('value_changed',self.cb_page_size,adj1)
		#scale = gtk.HScale(adj2)
		#scale.set_digits(0)
		#packit(box2,scale,True,True,0)
		#packit(box1,box2,True,True,0)
		
		#separator
		separator = gtk.HSeparator();
		packit(box1,separator,False,True,0)
		
		box2 = gtk.VBox(False,10)
		box2.set_border_width(10)
		packit(box1,box2,False,True,0)
		
		#quitButton
		button = gtk.Button('Quit')
		button.connect('clicked',lambda w: gtk.main_quit())
		button.set_flags(gtk.CAN_DEFAULT)
		packit(box2,button,True,True,0)
		button.grab_default()

		self.window.show()
		
if __name__ == "__main__":
	RangeWidget = Range()
	gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
