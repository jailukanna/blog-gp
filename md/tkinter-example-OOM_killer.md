---
title: tkinter example OOM killer (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'OOM killer'


Modules used in program: 
* `import psutil  # python -m pip install psutil`
* `import operator, os, subprocess, time`

## python OOM killer

Python tkinter example: OOM killer

```python
"""Out Of Memory (OOM) Killer
2018-02-08 v1.0 by Cees Timmerman
2018-04-18 v1.0.1 Fix for Windows.
2018-04-27 v1.0.2 Use process ID in Windows.
2018-05-01 v1.1 Account for HP's RAM tester.
2019-06-01 v1.2 Also alert and don't overwrite name.
"""
import operator, os, subprocess, time
#import tkinter as tk
#import tkinter.messagebox
import psutil  # python -m pip install psutil

MIN_BYTES_FREE = 240e6  # Windows 10 and/or Chrome appear to fill the disk rather than put free RAM below 20 MB of 12 GB. 128 MB is also hard to hit. 250e6 works.
# Case-sensitive list of process names that don't need confirmation to be killed.
HIT_LIST = ["chrome.exe", "firefox.exe", "MicrosoftEdgeCP.exe", "Spotify.exe", "python.exe"]

lines = __doc__.splitlines()
print(lines[0])
print(lines[-1])

'''
root = tk.Tk()
#root.overrideredirect(1)  # Breaks iconify.
root.withdraw()  # Hides GUI.
'''
while True:
	time.sleep(5)
	if psutil.virtual_memory().free < MIN_BYTES_FREE:
		totals = {}
		pids = {}
		for p in psutil.process_iter():
			name = p.name()
			if not name in totals: totals[name] = 0
			totals[name] += p.memory_percent()
			if not p.parent() or p.parent().name() != name: pids[name] = p.pid  # Chrome etc spawn multiple processes, so just kill their root.

		#print(p.name(), "using {0:.2f}% mem".format(p.memory_percent()))
		#hogs = sorted(totals, key=totals.get, reverse=True)  # Just names
		hogs = sorted(totals.items(), key=operator.itemgetter(1), reverse=True) # Name value pairs

		for hog in hogs[:3]: print("%r %.2f%%" % hog)

		name, percentage = hogs[0]
		pid = pids[name]
		ok = False
		if name in HIT_LIST:
			ok = True
			# Unless HP's overzealous RAM test is running. Don't overwrite name.
			for _name in totals:
				if 'pcdrmemory' in _name:
					ok = False
					break
		else:
			'''
			root.deiconify()  # Show icon in task bar.
			ok = tk.messagebox.askyesno("Low memory", "Windows might crash soon. Terminate %r to free %s%% RAM?" % (name, percentage)):
			root.withdraw()
			'''
		if ok:
			print("Killing", name, pid, "on", time.ctime(), "\7")  # Notify with audio as well.
			try:
				'''
				os.kill(pid, 15)  # 15 is SIGTERM according to https://en.wikipedia.org/wiki/Signal_(IPC) which is friendlier than SIGKILL (9). On Windows this is just the exit code for the process.
				print(os.waitpid(pid, 0x00000004)) # os.WEXITED, ignored on Windows.
				'''
				print(subprocess.check_output(["TASKKILL", "/PID", str(pid), "/T", "/F"]))  # /IM /F didn't work though all child processes were named the same.
			except Exception as ex:
				print(ex)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
