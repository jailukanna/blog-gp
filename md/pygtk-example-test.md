---
title: pygtk example test (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'test'

Functions in program: 
* `def main():`

Modules used in program: 
* `import threading`
* `import pynotify`
* `import appindicator`
* `import gobject`
* `import gtk`
* `import pygtk`
* `import pyinotify`
* `import signal`
* `import time`
* `import os`

## python test

Python pygtk example: test

```python
import os
import time
import signal
import pyinotify
import pygtk
pygtk.require('2.0')
import gtk
import gobject
import appindicator
import pynotify
import threading

gobject.threads_init()

class AppIndicatorExample:
  def __init__(self, watcher):
    self.ind = appindicator.Indicator ("example-simple-client", "indicator-messages", appindicator.CATEGORY_APPLICATION_STATUS)
    self.ind.set_status (appindicator.STATUS_ACTIVE)
    self.ind.set_attention_icon ("indicator-messages-new")
    self.ind.set_icon("distributor-logo")

    # save watcher
    self.watcher = watcher

    # create a menu
    self.menu = gtk.Menu()

    # create items for the menu - labels, checkboxes, radio buttons and images are supported:

    item = gtk.MenuItem("Regular Menu Item")
    item.show()
    self.menu.append(item)

    check = gtk.CheckMenuItem("Check Menu Item")
    check.show()
    self.menu.append(check)

    radio = gtk.RadioMenuItem(None, "Radio Menu Item")
    radio.show()
    self.menu.append(radio)

    image = gtk.ImageMenuItem(gtk.STOCK_QUIT)
    image.connect("activate", self.quit)
    image.show()
    self.menu.append(image)

    self.menu.show()

    self.ind.set_menu(self.menu)

  def quit(self, widget, data=None):
    self.watcher.halt()
    gtk.main_quit()

class EventHandler(pyinotify.ProcessEvent):
  def process_IN_DELETE(self, event):
    #pynotify.Notification("File deleted", "%s" %  os.path.join(event.path, event.name)).show()
    print("Deleted: %s" %  os.path.join(event.path, event.name))

  def process_IN_CREATE(self, event):
    #pynotify.Notification("File created", "%s" %  os.path.join(event.path, event.name)).show()
    print("Created: %s" %  os.path.join(event.path, event.name))

  def process_IN_MODIFY(self, event):
    #pynotify.Notification("File modified", "%s" %  os.path.join(event.path, event.name)).show()
    print("Modified: %s" %  os.path.join(event.path, event.name))

  def process_IN_DELETE_SELF(self, event):
    #pynotify.Notification("File deleted", "%s" %  os.path.join(event.path, event.name)).show()
    print("Deleted: %s" %  os.path.join(event.path, event.name))

  def process_IN_ATTRIB(self, event):
    #pynotify.Notification("File metadata changed", "%s" %  os.path.join(event.path, event.name)).show()
    print("Metadata: %s" %  os.path.join(event.path, event.name))


class Watcher(threading.Thread):
  def __init__(self):
    super(Watcher, self).__init__()
    self.wm = pyinotify.WatchManager()
    self.mask = ( pyinotify.IN_DELETE      |
                  pyinotify.IN_CREATE      |
                  pyinotify.IN_MODIFY      |
                  pyinotify.IN_DELETE_SELF |
                  pyinotify.IN_ATTRIB      )
    self.notifier = pyinotify.Notifier(self.wm, EventHandler())
    self.wdds = []
    self.quit = False

  def run(self):
    while not self.quit:
      self.notifier.process_events()
      if self.notifier.check_events():
        self.notifier.read_events()

  def add_watches(self):
    self.wdds.append(self.wm.add_watch('/home/rd/pedro.coelho/workspace/', self.mask, rec=True, auto_add=True))

  def halt(self):
    self.quit = True
    for i in self.wdds:
      self.wm.rm_watch(i.values())
    self.notifier.stop()

def main():
  if not pynotify.init("Basics"):
    sys.exit(1)

  watcher = Watcher()
  watcher.add_watches()
  watcher.start()

  indicator = AppIndicatorExample(watcher)

  gtk.main()
  return 0

if __name__ == "__main__":
  main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
