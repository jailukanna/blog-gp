---
title: tkinter example starthere (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'starthere'

Functions in program: 
* `def pklopen(filename, pkgcontent):`
* `def pklsave(filename, content):`
* `def selclickxwait(time, elem):`
* `def selelemxwait(time, elem):`
* `def selkeycss(sel, type):`
* `def selclickcss(sel):`
* `def selkeyxpath(xpath, type):`
* `def selclickxpath(xpath):`
* `def findclick(img):`
* `def clickfill(img, text):`
* `def switchToApp(app):`
* `def login_emailrak():`
* `def startChrome():`

Modules used in program: 
* `import urllib3`
* `import mechanicalsoup`
* `import cookiejar`
* `import tkinter as tk`
* `import re`
* `import math`
* `import appscript`
* `import tkinter as tk`
* `import time`
* `import Quartz`
* `import pandas as pd`
* `import numpy as np`
* `import array`
* `import pickle`
* `import glob`
* `import mouseinfo`
* `import pyautogui`
* `import time`
* `import _tkinter`
* `import requests`
* `import subprocess`
* `import os`

## python starthere

Python tkinter example: starthere

```python
import os
import subprocess
from typing import Dict
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
from selenium.common.exceptions import NoSuchElementException
from selenium.common.exceptions import StaleElementReferenceException
from selenium.common.exceptions import TimeoutException
import requests
from sympy import true
import _tkinter
import time
import pyautogui
import mouseinfo
import glob
import pickle
from pathlib import Path
import array
import numpy as np
from collections import OrderedDict
from operator import itemgetter
from datetime import datetime
import pandas as pd
from AppKit import NSApplication, NSApp, NSWorkspace
from Quartz import kCGWindowListOptionOnScreenOnly, kCGNullWindowID, CGWindowListCopyWindowInfo
import Quartz
import time
from Quartz import CGWindowListCopyWindowInfo, kCGWindowListExcludeDesktopElements, kCGNullWindowID
from Foundation import NSSet, NSMutableSet
import tkinter as tk
import appscript
import math
from urllib.parse import urljoin
import re
import tkinter as tk
from bs4 import BeautifulSoup
from lxml import html
import cookiejar
import mechanicalsoup
import urllib3
from http import cookiejar

CHROMEDRIVER = '/Users/olivia/Downloads/chromedriver'

def startChrome():
	options = webdriver.ChromeOptions()
	options.add_experimental_option(
		'excludeSwitches',
		['disable-hang-monitor',
		 'disable-prompt-on-repost',
		 'disable-background-networking',
		 'disable-sync',
		 'disable-translate',
		 'disable-web-resources',
		 'disable-client-side-phishing-detection',
		 'disable-component-update',
		 'disable-default-apps',
		 'disable-zero-browsers-open-for-tests'])
	options.add_argument("--user-data-dir=/Users/olivia/Library/Application Support/Google/Chrome")
	options.add_argument("--profile-directory=Profile 2")
	# options.add_argument("--headless")
	options.binary_location = '/Applications/Google Chrome 79.app/Contents/MacOS/Google Chrome'
	options.add_argument("--enable-local-sync-backend")
	# options.binary_location = '/Applications/Google Chrome Canary.app/Contents/MacOS/Google Chrome Canary'
	# options.add_argument("--disable-extensions")
	driver = webdriver.Chrome(CHROMEDRIVER, options=options)
	driver.set_page_load_timeout(10)
	driver.maximize_window()
	return driver

# driver = startChrome()

def login_emailrak():
	try:
		element = WebDriverWait(driver, 20).until(
			EC.presence_of_element_located((By.XPATH, '//*[@id="signin_email"]'))
			)
	finally:
		rakemail = driver.find_element_by_xpath('/*[@id="signin_email"]')
		rakemail.send_keys("olivialau@live.ca")
		time.sleep(1)
		rakpw = driver.find_element_by_xpath('//*[@id="signin_password"]')
		rakpw.send_keys("1994930Ol:)")
		time.sleep(1)
		raksubmit = driver.find_element_by_xpath('//*[@id="button-login"]')
		raksubmit.click()
def switchToApp(app):
	appscript.app(app).activate()
## PYAUTO ---------------------------------------------------------------------------------
## PYAUTO ---------------------------------------------------------------------------------
## PYAUTO ---------------------------------------------------------------------------------
def clickfill(img, text):
	# for x in range(10):
	#     if pyautogui.locateOnScreen(img):
	#         pin = x
	#         pass
	#     else:
	#         time.sleep(x)
	print(pyautogui.locateOnScreen(img))
	pyautogui.click(pyautogui.locateOnScreen(img))
	pyautogui.write(text)
def findclick(img):
	# for x in range(10):
	#     if pyautogui.locateOnScreen(img):
	#         pin = x
	#         pass
	#     else:
	#         time.sleep(x)
	pyautogui.click(pyautogui.locateOnScreen(img))
## SEL ---------------------------------------------------------------------------------
## SEL ---------------------------------------------------------------------------------
## SEL ---------------------------------------------------------------------------------
def selclickxpath(xpath):
	m = driver.find_element_by_xpath(xpath)
	m.click()
def selkeyxpath(xpath, type):
	m = driver.find_element_by_xpath(xpath)
	m.send_keys(type)
def selclickcss(sel):
	m = driver.find_element_by_xpath(sel)
	m.click()
def selkeycss(sel, type):
	m = driver.find_element_by_xpath(sel)
	m.send_keys(type)
def selelemxwait(time, elem):
    element = WebDriverWait(driver, time).until(
        EC.presence_of_element_located((By.XPATH, elem))
    )
def selclickxwait(time, elem):
    element = WebDriverWait(driver, time).until(
        EC.element_to_be_clickable((By.XPATH, elem))
    )
## PKL ---------------------------------------------------------------------------------
## PKL ---------------------------------------------------------------------------------
## PKL ---------------------------------------------------------------------------------
def pklsave(filename, content):
	with open(str(filename) + '.pkl', 'wb') as f:
		pickle.dump(content, f)
def pklopen(filename, pkgcontent):
	with open(str(filename) + '.pkl', 'rb') as f:
		pkgcontent = pickle.load(f)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
