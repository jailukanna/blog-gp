---
title: pil example mjpgserver class (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'mjpgserver class'

Functions in program: 
* `def pull():`
* `def pull():`
* `def pull():`

Modules used in program: 
* `import gevent`

## python mjpgserver class

Python pil example: mjpgserver class

```python
#!/usr/bin/python3.4
"""

Made by: David Smerkous - smerkousdavid@gmail.com
A simple(class) mjpegserver written in python, for ease of use
on any video streaming application or testing

License: GPL3+
Python version: 3.4+ (Could easily be transferred to <3.4 with some changes)

Original post/idea: https://gist.github.com/n3wtron/4624820
Made for: I decided to share this because maybe another FRC
team that needs a python versionized mjpeg streamer for their robot... In
short just an idea thrown together by me for team 5431: https://github.com/frc5431

"""

from __future__ import print_function

from urllib.parse import parse_qs

from gevent import monkey
import gevent

monkey.patch_socket()
monkey.patch_dns()
monkey.patch_time()
monkey.patch_subprocess()

from http.server import BaseHTTPRequestHandler, HTTPServer
from socketserver import ThreadingMixIn
from time import sleep, time
from io import BytesIO
from threading import Thread

try:
    import cv2
except ImportError:
    pass


# noinspection PyBroadException
class MjpgServer(BaseHTTPRequestHandler):
    def __init__(self, bind_addr='localhost', port=8080, name='cam'):
        class THTTPS(ThreadingMixIn, HTTPServer):
            pass

        self._h_server = THTTPS((bind_addr, port), super().__init__)
        self._name = str(name)
        self._port = port
        self._bind = bind_addr
        self._ext_types = ('.mjpg', '.html', '.jpeg', 'fps', 'quality')
        self._methods = ('open', 'pil', 'raw')
        self._cur_method = 'open'
        self._pull_method = None
        self._fps = -1
        self._real_fps = 0
        self._measure_fps = False
        self._cur_size = 1
        self._quality = -1

    def set_fps(self, fps=-1):
        self._fps = int(fps)

    def set_quality(self, quality=-1):
        self._quality = quality

    def get_fps(self):
        self._measure_fps = True
        return self._real_fps

    def get_band_usage(self):
        return ((self._cur_size / 1024) / 1024) * self.get_fps()

    def attach_opencv(self, method):
        self._pull_method = method
        self._cur_method = self._methods[0]

    def attach_pil(self, method):
        self._pull_method = method
        self._cur_method = self._methods[1]

    def attach_raw(self, method):
        self._pull_method = method
        self._cur_method = self._methods[2]

    def start(self, threaded=False):
        if self._pull_method is None:
            raise RuntimeError('Request method not found')

        print("Starting mjpeg server http://%s:%d/%s.html" % (self._bind, self._port, self._name))
        if not threaded:
            self._h_server.serve_forever()
            return None
        else:
            mjpg_thread = Thread(target=self._h_server.serve_forever)
            mjpg_thread.setDaemon(True)
            mjpg_thread.start()
            return mjpg_thread

    def __write(self, message):
        if isinstance(message, str):
            self.wfile.write(message.encode('utf8'))
        else:
            self.wfile.write(message)

    def __get_cv_encoded(self):
        ok, img = self._pull_method()
        if not ok:
            return None
        rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        if self._quality > -1:
            im_enc = cv2.imencode('.jpeg', rgb, [cv2.IMWRITE_JPEG_QUALITY, self._quality])[1]
        else:
            im_enc = cv2.imencode('.jpeg', rgb)[1]
        enc = bytearray(im_enc)
        return enc

    def __get_pil_encoded(self):
        ok, img = self._pull_method()
        if not ok:
            return None
        out_stream = BytesIO()
        if self._quality > -1:
            img.save(out_stream, 'JPEG', quality=self._quality)
        else:
            img.save(out_stream, 'JPEG')
        return out_stream.getvalue()

    def __get_raw_encoded(self):
        ok, img = self._pull_method()
        if not ok:
            return None
        out_stream = BytesIO()
        if isinstance(img, str):
            out_stream.write(img.encode('utf8'))
        elif isinstance(img, (bytearray, bytes)):
            out_stream.write(img)
        return out_stream.getvalue()

    def __sel_enc(self):
        if self._cur_method == self._methods[0]:
            enc = self.__get_cv_encoded()
        elif self._cur_method == self._methods[1]:
            enc = self.__get_pil_encoded()
        else:
            enc = self.__get_raw_encoded()
        return enc

    def __handle_mjpeg(self):
        global cap
        try:
            self.send_response(200)
            self.send_header('Content-type', 'multipart/x-mixed-replace; boundary=--jpgboundary')
            self.end_headers()
        except Exception:
            return
        stime = time()
        cfps_time = time()
        count_to_fps = 0
        while 1:
            if self._fps != -1:
                ref_time = 0.95 / self._fps
                if (time() - stime) >= ref_time:
                    stime = time() - 0.1
                else:
                    gevent.sleep(ref_time / 1.1)
            try:
                enc = self.__sel_enc()
                if not enc:
                    continue
                if self._measure_fps:
                    count_to_fps += 1
                    if count_to_fps >= self._fps:
                        t_to_fps = time() - cfps_time
                        self._real_fps = 1.0 / t_to_fps
                        cfps_time = time()
                        count_to_fps = 0
                self.wfile.write(b'--jpgboundary')
                self.send_header('Content-type', 'image/jpeg')
                self._cur_size = len(enc)
                self.send_header('Content-length', self._cur_size)
                self.end_headers()
                self.wfile.write(enc)
            except Exception:
                break

    def __handle_html(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        self.wfile.write(("<html><head></head><body><img src=\"http://127.0.0.1:%d/%s.mjpg\"/></body></html>" %
                          (self._port, self._name)).encode('utf8'))

    def __handle_jpeg(self):
        enc = self.__sel_enc()
        if enc is None:
            self.send_error(505, 'Error parsing image')
        self.send_response(200)
        self.send_header('Content-type', 'image/jpeg')
        self.send_header('Content-length', len(enc))
        self.end_headers()
        self.wfile.write(enc)

    def do_POST(self):
        """
        Do a post request to the server to
        set settings such as quality and fps
        """
        length = int(self.headers['Content-Length'])

        post_data = parse_qs(self.rfile.read(length).decode('utf-8'))
        for key, value in post_data.items():
            key = key.lower()
            if key == 'fps':
                self._fps = int(value[0])
                print("Setting new fps: %s" % self._fps)
            elif key == 'quality':
                self._quality = int(value[0])
                print("Setting new quality: %s" % self._quality)
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()

    def do_GET(self):
        ret_path = self.path

        if ret_path.endswith(self._name + self._ext_types[0]):
            self.__handle_mjpeg()
        elif ret_path.endswith(self._name + self._ext_types[1]) or self.path == "/":
            self.__handle_html()
        elif ret_path.endswith(self._name + self._ext_types[2]):
            self.__handle_jpeg()
        elif ret_path.endswith(self._ext_types[3]):
            self.send_response(200)
            self.send_header('Content-type', 'text/plain')
            self.end_headers()
            self.wfile.write(("%d" % self._fps).encode('utf8'))
        elif ret_path.endswith(self._ext_types[4]):
            self.send_response(200)
            self.send_header('Content-type', 'text/plain')
            self.end_headers()
            self.wfile.write(("%d" % self._quality).encode('utf8'))
        else:
            self.send_error(404, "This server only accepts %s(%s, %s, %s)" % (self._name, self._ext_types[0],
                                                                              self._ext_types[1],
                                                                              self._ext_types[2]))
        return

'''
Opencv capture example
'''
cap = cv2.VideoCapture(0)
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1920)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 1080)
cap.set(cv2.CAP_PROP_SATURATION, 0.5)


def pull():
    global cap
    return cap.read()

'''
PIL
arr = '\x30\x12...'

def pull():
   img = Image.fromarray(arr)
   ok = img is not None
   return ok, img
'''

'''
RAW (Byte array or string)

def pull():
    img = bytearray(arr)
    ok = False
    if len(img) > 0:
        ok = True
    return ok, img

'''

if __name__ == '__main__':
    mjpg_server = MjpgServer(port=8080, name="cam")
    mjpg_server.attach_opencv(pull)
    mjpg_server.set_quality(50)
    mjpg_server.start(threaded=True)  # Remove if you want it to be the only thing running

    # While loop to eat the space away
    while True:
        print("FPS: %d" % mjpg_server.get_fps())  # Get 'real' fps (Calculated pulling)
        print("BAND(B): %.2f" % (mjpg_server.get_band_usage()))  # Get calculated bandwith (p.s. it gets
                                                                 # image size and real fps to calc)
        sleep(5)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
