---
title: pygtk example minify (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'minify'


Modules used in program: 
* `import urllib2 as urllib`
* `import json`
* `import platform`

## python minify

Python pygtk example: minify

```python
#!/usr/bin/env python

import platform
import json
import urllib2 as urllib
from urllib import urlencode

# ---
# Copy + Paste functions

os_type = platform.system()

if os_type == "Windows":
	import win32clipboard as clip
	
	def cb_read():
		clip.OpenClipboard()
		try:
			text = clip.GetClipboardData()
		except Exception, e:
			text = False
		clip.CloseClipboard()
		return text
	
	def cb_write(text):
		clip.OpenClipboard()
		clip.SetClipboardText(text)
		clip.SetClipboardText(text, clip.CF_UNICODETEXT)
		clip.CloseClipboard()

elif os_type == "Linux":
	import pygtk
	pygtk.require("2.0")
	import gtk
	clip = gtk.clipboard_get()
	
	def cb_read():
		return clip.wait_for_text()
	
	def cb_write(text):
		clip.set_text(text)
		clip.store()

elif os_type == "Darwin":
	import subprocess as sub
	
	def cb_read():
		p1 = sub.Popen(['pbpaste'], stdout=sub.PIPE)
		p2 = sub.Popen(['cat'], stdin=p1.stdout, stdout=sub.PIPE)
		p1.stdout.close()
		return p2.communicate()[0]
	
	def cb_write(text):
		p1 = sub.Popen(['echo', text], stdout=sub.PIPE)
		p2 = sub.Popen(['pbcopy'], stdin=p1.stdout, stdout=sub.PIPE)
		p1.stdout.close()

# ---

if __name__ == "__main__":
	# Read clipboard
	data = cb_read()
	
	# Validate
	if not data:
		print("Empty clipboard. Closing.")
		exit()
	
	data = data.strip()
	
	if not data.startswith('http'):
		print("No URL in clipboard. Closing.")
		exit()
	
	print("Link read from clipboard >>", data)
	
	api_url = 'https://www.googleapis.com/urlshortener/v1/url?key=AIzaSyB9BI7NsyLDrHljRKkW0gFOwQcqqoQOVdM'
	
	# Build POST request
	request = urllib.Request(
		url = api_url,
		data = '{"longUrl":"%s"}' % data,
		headers = {'Content-Type': 'application/json'}
	)
	
	# Send request
	try:
		result = urllib.urlopen(request)
	except Exception, e:
		print("=== ERROR ===")
		print(e.read())
		exit()
	
	# Parse response
	response = json.loads(result.read())
	minified = response['id'].strip()
	
	# Write to clipboard
	cb_write(minified)
	
	print("Minified link copied to clipboard >>", minified)
	exit()

# end (:

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
