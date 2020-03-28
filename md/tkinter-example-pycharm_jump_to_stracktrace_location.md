---
title: tkinter example pycharm jump to stracktrace location (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'pycharm jump to stracktrace location'

Functions in program: 
* `def get_last_error_location(stacktrace=None):`
* `def get_open_pycharm_projects():`
* `def notify(msg):`

## python pycharm jump to stracktrace location

Python tkinter example: pycharm jump to stracktrace location

```python

"""
Get stacktrace from the clipboard, and open the relevant file/line in the 
current PyCharm project (useful when using terminal instead of PyCharm run).

Arguments:
1. The path to the PyCharm launcher script (e.g. "~/pycharm/bin/pycharm.sh")
2. [Optional] A json dictionary of path substitutions (e.g '{"/remote/userA": "/home/userA"}')

https://www.jetbrains.com/help/pycharm/2016.3/opening-files-from-command-line.html
"""


from os import listdir, environ
from re import findall
from sys import stderr, argv
from os.path import join, exists
from bs4 import BeautifulSoup
from subprocess import Popen
from json import loads

try:
	import Tkinter as tk
except ImportError:
	import tkinter as tk



def notify(msg):
    stderr.write('{0:s}\n'.format(msg))
    Popen(['notify-send', "{0:s}".format(msg)])


def get_open_pycharm_projects():
	homedir = join('/home', environ.get('USER'))
	li = listdir(homedir)
	sli = sorted(pth for pth in li if pth.startswith('.PyCharm'))
	if len(sli) == 0:
		notify('did not find pycharm config')
		exit(1)
	conf_root = sli[-1]
	recent_pth = join(homedir, conf_root, 'config', 'options', 'recentProjectDirectories.xml')
	with open(recent_pth, 'r') as fh:
		conf = BeautifulSoup(fh, 'html.parser')
	open_projs = conf.find('option', dict(name='openPaths')).list.find_all('option')
	return tuple(opt['value'].replace('$USER_HOME$', homedir) for opt in open_projs)

if len(argv) <= 1:
	notify('first argument should be pycharm launch script')
	exit(2)
pycharm_pth = argv[1]
if not exists(pycharm_pth):
	notify('pycharm path not found "{0:s}"'.format(pycharm_pth))
	exit(3)

pth_substitutions = {}
if len(argv) >= 3:
	pth_substitutions = loads(argv[2])

open_projs = get_open_pycharm_projects()
if len(open_projs) == 0:
	notify('did not find open pycharm projects')
	exit(4)
open_proj = open_projs[0]
if len(open_projs) > 1:
	notify('found {0:d} open projects; choosing {1:s}'.format(len(open_projs), open_proj))

def get_last_error_location(stacktrace=None):
	if stacktrace is None:
		root = tk.Tk()
		root.withdraw()
		stacktrace = root.clipboard_get()
	if 'File' not in stacktrace:
		notify('did not find a python stacktrace')
		exit(5)
	steps = findall(r'File "([^"\n]+)", line (\d+), in', stacktrace)
	for step in reversed(steps):
		if '-packages' not in step[0]:
			return step
	notify('did not find any paths in stacktrace that don\'t have "-packages" in their path')
	exit(6)

file_pth, linenr = get_last_error_location()
for frm, to in pth_substitutions.items():
	file_pth = file_pth.replace(frm, to)


notify('"{0:s}" "{1:s}" --line {3:s} "{2:s}"'.format(pycharm_pth, open_proj, file_pth, linenr))
Popen([pycharm_pth, open_proj, '--line', linenr, file_pth])




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
