---
title: tkinter example xianmian (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'xianmian'

Functions in program: 
* `def center_window(root, width, height):`
* `def get_window_size(window):`
* `def get_screen_size(window):`

Modules used in program: 
* `import json`
* `import Tkinter`
* `import scrapy`

## python xianmian

Python tkinter example: xianmian

```python
# -*- coding: utf-8 -*-
import scrapy
import Tkinter
from scrapy.shell import inspect_response
import json

# 设置感兴趣的app名称
I_want_apps = set(['sanedesk'])

class XianmianSpider(scrapy.Spider):
    user_agent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36'
    name = "xianmian"
    allowed_domains = ["app.so"]
    start_urls = (
        'http://app.so/api/v1.1/appso/discount/?platform=web&limit=10',
    )

    def parse(self, response):

        jsonresponse = json.loads(response.body_as_unicode())

        apps = jsonresponse['objects']

        appTitles = {item['display_name'].lower() for item in apps}

        self.logger.info('today\' apps are: ' + str(appTitles))
        # inspect_response(response, self)
        the_apps = appTitles & I_want_apps
        if the_apps:
        	self.showMsg('found the apps: {}'.format(list(the_apps)))

    def showMsg(self, msg):
    	import Tkinter
    	root = Tkinter.Tk()
    	root.title('福利到！')
    	label = Tkinter.Label(root, text=msg)
    	label.pack()
    	center_window(root, 300, 240)
    	root.maxsize(600, 400)
    	root.minsize(300, 240)
    	Tkinter.mainloop()




def get_screen_size(window):
    return window.winfo_screenwidth(),window.winfo_screenheight()

def get_window_size(window):
    return window.winfo_reqwidth(),window.winfo_reqheight()

def center_window(root, width, height):
    screenwidth = root.winfo_screenwidth()
    screenheight = root.winfo_screenheight()
    size = '%dx%d+%d+%d' % (width, height, (screenwidth - width)/2, (screenheight - height)/2)
    print(size)
    root.geometry(size)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
