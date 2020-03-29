---
title: pygtk example winotify (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'winotify'

Functions in program: 
* `def _set_urgency_hint(window, activate):`
* `def on_focus(window, changed_mask, new_state):`
* `def register_window_handler(window_name):`
* `def start_server(port):`
* `def main(args):`

Modules used in program: 
* `import wnck`
* `import gtk`
* `import pygtk`
* `import socket`
* `import sys`

## python winotify

Python pygtk example: winotify

```python
#!/usr/bin/env python

"""
highlight GTK window

Usage:
  $ winotify.py <window title>

listens to notifications on port 10001
"""

import sys
import socket

import pygtk
pygtk.require("2.0")
import gtk
import wnck

from time import sleep


PORT = 10001

TARGET_WINDOW = None


def main(args):
	args = [unicode(arg, "utf-8") for arg in args]
	window_name = args[1]

	global TARGET_WINDOW # required for event handler -- XXX: really?
	TARGET_WINDOW = register_window_handler(window_name)

	server = start_server(PORT)

	while True:
		while gtk.events_pending():
			gtk.main_iteration(block=False)
		try:
			connection, client = server.accept()
		except socket.error:
			sleep(1)
			continue
		try:
			while True:
				data = connection.recv(16) # XXX: excessive length? (required is only a ping or binary flag)
				if data:
					_set_urgency_hint(TARGET_WINDOW, True)
				else:
					break
		finally:
			connection.close()

	return True


def start_server(port):
	server = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # TODO: should use AF_UNIX
	server.setblocking(0)
	server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
	server_address = ("localhost", port)
	server.bind(server_address)
	server.listen(1)
	return server


def register_window_handler(window_name):
	screen = wnck.screen_get_default()
	# process pending GTK events to include existing windows
	while gtk.events_pending():
		gtk.main_iteration()
	# register event handler
	for window in screen.get_windows():
		if window.get_name() == window_name:
			xid = window.get_xid()
			wrapper = gtk.gdk.window_foreign_new(xid)
			wrapper.iconify() # required to trigger state change on focus -- XXX: really?
			window.connect("state-changed", on_focus) # XXX: state-change less than ideal
	return wrapper


def on_focus(window, changed_mask, new_state):
	if not "urgent" in changed_mask.value_nicks:
		_set_urgency_hint(TARGET_WINDOW, False)


def _set_urgency_hint(window, activate):
	"""
	low-level replacement for set_urgency_hint method, which caused segfault
	"""
	_type, _format, data = window.property_get("WM_HINTS")

	# 1<<8 == XUrgencyHint
	if activate:
		data[0] |= 256
	else: # deactivate
		data[0] &= ~256

	window.property_change("WM_HINTS", _type, _format,
			gtk.gdk.PROP_MODE_REPLACE, data)
	gtk.gdk.window_process_all_updates()


if __name__ == "__main__":
	status = not main(sys.argv)
	sys.exit(status)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
