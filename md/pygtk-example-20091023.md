---
title: pygtk example 20091023 (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example '20091023'


Modules used in program: 
* `import sys`

## python 20091023

Python pygtk example: 20091023

```python
#!/usr/bin/env python
#calc
import sys
try:
	import pygtk
	pygtk.require("2.0")
except:
	pass
try:
	import gtk
	import gtk.glade
except:
	print("This program need to be run in graphical environment!")
	sys.exit(1)

class calc:
	def __init__(self):
		self.level = 1
		self.method = 1
		self.value1 = 0
		self.value2 = 0
		self.gladefile = "./calc.glade"
		self.wTree = gtk.glade.XML(self.gladefile)
		dic = { "on_window1_destroy" : gtk.main_quit,
			"on_button1_clicked" : self.button1_clicked,
			"on_button2_clicked" : self.button2_clicked,
			"on_button3_clicked" : self.button3_clicked,
			"on_button4_clicked" : self.button4_clicked,
			"on_button5_clicked" : self.button5_clicked,
			"on_button6_clicked" : self.button6_clicked,
			"on_button7_clicked" : self.button7_clicked,
			"on_button8_clicked" : self.button8_clicked,
			"on_button9_clicked" : self.button9_clicked,
			"on_button0_clicked" : self.button0_clicked,
			"on_buttonadd_clicked" : self.buttonadd_clicked,
			"on_buttonsub_clicked" : self.buttonsub_clicked,
			"on_buttonsum_clicked" : self.buttonsum_clicked }
		self.wTree.signal_autoconnect(dic)
		self.label = self.wTree.get_widget("label")
		self.wTree.get_widget("window1").show()

	def button1_clicked(self, widget):
		if self.level == 1:
			self.value1 = 1
		elif self.level == 2:
			self.value2 = 1
	def button2_clicked(self, widget):
		if self.level == 1:
			self.value1 = 2
		elif self.level == 2:
			self.value2 = 2
	def button3_clicked(self, widget):
		if self.level == 1:
			self.value1 = 3
		elif self.level == 2:
			self.value2 = 3
	def button4_clicked(self, widget):
		if self.level == 1:
			self.value1 = 4
		elif self.level == 2:
			self.value2 = 4
	def button5_clicked(self, widget):
		if self.level == 1:
			self.value1 = 5
		elif self.level == 2:
			self.value2 = 5
	def button6_clicked(self, widget):
		if self.level == 1:
			self.value1 = 6
		elif self.level == 2:
			self.value2 = 6
	def button7_clicked(self, widget):
		if self.level == 1:
			self.value1 = 7
		elif self.level == 2:
			self.value2 = 7
	def button8_clicked(self, widget):
		if self.level == 1:
			self.value1 = 8
		elif self.level == 2:
			self.value2 = 8
	def button9_clicked(self, widget):
		if self.level == 1:
			self.value1 = 9
		elif self.level == 2:
			self.value2 = 9
	def button0_clicked(self, widget):
		if self.level == 1:
			self.value1 = 0
		elif self.level == 2:
			self.value2 = 0
	def buttonadd_clicked(self, widget):
		self.level = 2
		self.method = 1
	def buttonsub_clicked(self, widget):
		self.level = 2
		self.method = 2
	def buttonsum_clicked(self, widget):
		self.level = 1
		if self.method == 1:
			self.value3 = self.value1 + self.value2
		elif self.method == 2:
			self.value3 = self.value1 - self.value2
		self.label.set_text(str(self.value3))
		self.value1 = 0
		self.value2 = 0


if __name__ == "__main__":
	frm = calc()
	gtk.main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
