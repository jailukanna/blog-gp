---
title: pygtk example choosealbumart (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'choosealbumart'


Modules used in program: 
* `import gtk`
* `import pygtk`
* `import subprocess`
* `import sys`
* `import os`

## python choosealbumart

Python pygtk example: choosealbumart

```python
import os
import sys
import subprocess
import pygtk
import gtk

class Chooser:
	def __init__(self):
		self.win = gtk.Window(gtk.WINDOW_TOPLEVEL)
		self.win.connect("delete_event", self.quit)
		self.win.connect("destroy", self.quit)

		vbox = gtk.VBox()
		self.win.add(vbox)

		self.path = gtk.Label()
		vbox.pack_start(self.path, expand=False)

		self.hbox = gtk.HBox()
		vbox.pack_start(self.hbox, expand=True)

		self.progress = gtk.ProgressBar()
		self.progress.set_fraction(0)
		vbox.pack_end(self.progress, expand=False)

		skip = gtk.Button("skip")
		def skip_action(*args, **kwargs):
			self.nextchoice()
		skip.connect("clicked", skip_action)
		vbox.pack_end(skip, expand=False)

		self.win.show_all()

		self.walk()

		self.done = 0
		self.nextchoice()

	def run(self):
		gtk.main()

	def walk(self):
		self.choices = []
		for dirname, dirs, files in os.walk("/home/bjn/media/music"):
			covers = []
			for file in files:
				if file.startswith("cover"):
					covers.append(file)
			if len(covers) > 1:
				self.choices.append((dirname, covers))

	def nextchoice(self):
		self.done += 1
		if self.done == len(self.choices):
			self.quit()
		self.progress.set_fraction(float(self.done) / float(len(self.choices)))
		self.choice(self.choices[self.done])

	def choice(self, choice):
		dirname, covers = choice
		first = True
		for w in self.hbox.get_children():
			w.destroy()
		self.path.set_text(dirname)
		for cover in covers:
			if not first:
				self.hbox.pack_start(gtk.VSeparator(), expand=False)
			else:
				first = False

			vbox = gtk.VBox()
			self.hbox.pack_start(vbox)

			path = os.path.join(dirname, cover)
			vbox.pack_start(gtk.Label(cover), expand=False)

			vbox.pack_start(gtk.Label(self.md5sum(path)), expand=False)

			data = self.imagedata(path)

			for key in ["Format", "Geometry", "Type", "Depth", "Quality"]:
				try:
					vbox.pack_start(gtk.Label(key + ": " + data[key]), expand=False)
				except KeyError:
					pass

			im = gtk.image_new_from_file(path)
			vbox.pack_start(im, expand=True)

			but = gtk.Button("This one")
			but.connect("clicked", self.choose)
			vbox.pack_end(but, expand=False)

		self.hbox.show_all()

	def choose(self, but):
		chosen = but.get_parent().get_children()[0].get_text()
		dirname, covers = self.choices[self.done]
		chosenpath = os.path.join(dirname, chosen)

		data = self.imagedata(chosenpath)
		if data["Format"].startswith("PNG"):
			extension = "png"
		elif data["Format"].startswith("JPEG"):
			extension = "jpg"
		elif data["Format"].startswith("GIF"):
			extension = "gif"
		else:
			dialog = gtk.MessageDialog(
					parent=self.win,
					buttons=gtk.BUTTONS_CANCEL, type=gtk.MESSAGE_ERROR,
					message_format="Unknown format '%s', no action taken"
							% data["Format"])
			dialog.run()
			dialog.destroy()
			return

		# delete the unchosen
		for cover in covers:
			if cover != chosen:
				deletepath = os.path.join(dirname, cover)
				print("DELETE '%s'" % deletepath)
				os.remove(deletepath)

		# rename the chosen
		newfilename = "cover.%s" % extension
		destinationpath = os.path.join(dirname, newfilename)
		if newfilename == chosen:
			print("filename '%s' is already correct" % newfilename)
		else:
			print("MOVE '%s' to '%s'" % (chosenpath, destinationpath))
			os.rename(chosenpath, destinationpath)

		# next choice
		self.nextchoice()

	def md5sum(self, path):
		return subprocess.Popen(["md5sum", path],
				stdout=subprocess.PIPE).communicate()[0].split(" ")[0]

	def imagedata(self, path):
		info = subprocess.Popen(["identify", "-verbose", path],
				stdout=subprocess.PIPE).communicate()[0].split("\n")
		# TODO: this could be parsed in a more sophisticated way
		data = {}
		for line in info:
			try:
				key, value = line.split(":", 1)
				data[key.strip()] = value.strip()
			except ValueError:
				pass
		return data

	def quit(self, *args, **kwargs):
		gtk.main_quit()
		sys.exit()

if __name__ == "__main__":
	chooser = Chooser()
	chooser.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
