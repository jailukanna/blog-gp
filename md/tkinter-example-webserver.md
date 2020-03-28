---
title: tkinter example webserver (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'webserver'

Functions in program: 
* `def st_server():`

Modules used in program: 
* `import json`
* `import sqlite3`
* `import _thread as thread`
* `import http.server as httpserver`
* `import tkinter.scrolledtext as tkscrolledtext`
* `import tkinter as tk`

## python webserver

Python tkinter example: webserver

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
""" DMLogger """
import tkinter as tk
import tkinter.scrolledtext as tkscrolledtext
import http.server as httpserver
import _thread as thread
import sqlite3
import json
from urllib import parse
from datetime import datetime


def st_server():
    """Start server"""
    while True:
        httpd.handle_request()


class MyHandler(httpserver.BaseHTTPRequestHandler):
    def do_HEAD(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()

    def do_GET(self):
        datalogger_path = '/ruta-----'
        if self.path.startswith(datalogger_path):
            parseResult = parse.urlparse(self.path)
            query = parse.parse_qs(parseResult.query)
            ####
            self.respond({'status': 200})
        else:
            self.respond({'status': 500})

    def handle_http(self, status_code, path):
        self.send_response(status_code)
        self.send_header('Content-type', 'application/json')
        self.end_headers()
        content = json.dumps({"status": "ok"})
        return bytes(content, 'UTF-8')

    def respond(self, opts):
        response = self.handle_http(opts['status'], self.path)
        self.wfile.write(response)


class Application(tk.Frame):
    def start_database(self):
        ####

    def start_server(self):
        thread.start_new_thread(st_server, ())
        self.text.insert('end', "Servidor iniciado en el puerto: {}\n".format(PORT))

    def createWidgets(self):
        """create GUI Tkinter"""

        # config database
        self.start_database()

        # exit
        self.QUIT = tk.Button(self)
        self.QUIT["text"] = "Finalizar"
        self.QUIT["fg"] = "red"
        self.QUIT["command"] = self.quit
        self.QUIT.pack({"side": "top", "fill": "x"})

        # Information
        self.lab = tk.Label(self, text="Información")
        self.lab.pack({"side": "top"})

        self.text = tkscrolledtext.ScrolledText(self)
        self.text["width"] = 40
        self.text["height"] = 5
        self.text.pack({"side": "left"})

    def __init__(self, master=None):
        tk.Frame.__init__(self, master)
        self.pack(expand='yes')
        self.createWidgets()
        self.start_server()


PORT = 8080
Handler = httpserver.HTTPServer
httpd = httpserver.HTTPServer(("", PORT), MyHandler)
root = tk.Tk()
root.after(0, sincroniza_servidor)
root.title("Datalogger")
app = Application(master=root)
app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
