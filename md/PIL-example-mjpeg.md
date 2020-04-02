---
title: PIL example mjpeg (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'mjpeg'

Functions in program: 
* `def get_handler(camera, frame_sleep):`
* `def get_frame(camera):`

Modules used in program: 
* `import pygame.camera`
* `import pygame`
* `import PIL.Image`
* `import threading`
* `import SocketServer`
* `import SimpleHTTPServer`
* `import BaseHTTPServer`
* `import time`

## python mjpeg

Python PIL example: mjpeg

```python
#!/usr/bin/env python2

import time

# HTTP
import BaseHTTPServer
import SimpleHTTPServer
import SocketServer
import threading

# Camera
from cStringIO import StringIO
import PIL.Image
import pygame
import pygame.camera


class ThreadedHTTPServer(SocketServer.ThreadingMixIn, BaseHTTPServer.HTTPServer):
	"""Handle requests in a separate thread."""
	daemon_threads = True

def get_frame(camera):
	"""Gets a jpg from the camera as a string"""

	# Get a non-jpg string representation
	stringimg = pygame.image.tostring(camera.get_image(), "RGBA", False)

	# Convert to a PIL image
	img = PIL.Image.frombytes("RGBA", camera.get_size(), stringimg)

	# Save a JPEG to a StringIO
	sio = StringIO()
	img.save(sio, 'JPEG')
	jpg = sio.getvalue()
	sio.close()

	return jpg

def get_handler(camera, frame_sleep):
	class Handler(SimpleHTTPServer.SimpleHTTPRequestHandler):
		def do_GET(self):
			"""Serve a GET request."""
			self.send_response(200)
			self.send_header('Content-Type', 'multipart/x-mixed-replace;boundary=boundarydonotcross')
			self.end_headers()

			while True:
				jpg = get_frame(camera)
				self.wfile.write('--boundarydonotcross\r\n')
				self.send_header('Content-type', 'image/jpeg')
				self.send_header('Content-length', len(jpg))
				self.end_headers()
				self.wfile.write(jpg + '\r\n')
				time.sleep(frame_sleep)
	return Handler


if __name__ == '__main__':
	host = '0.0.0.0'
	port = 8080
	size = (800, 600)
	frame_sleep = 1.0 / 5 # 15 fps

	# Get camera
	pygame.camera.init()
	cameras = pygame.camera.list_cameras()
	cam = pygame.camera.Camera(cameras[0], size)
	cam.start()

	# Get webserver
	handler = get_handler(cam, frame_sleep)
	httpd = ThreadedHTTPServer((host, port), handler)

	# Run websever
	print("Running on port {}".format(port))
	httpd.serve_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
