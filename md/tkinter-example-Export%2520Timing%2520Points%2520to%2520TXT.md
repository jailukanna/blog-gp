---
title: tkinter example Export%2520Timing%2520Points%2520to%2520TXT (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Export%2520Timing%2520Points%2520to%2520TXT'

Functions in program: 
* `def export(file_path):`
* `def check_file_path():`
* `def update_txt_path():`

Modules used in program: 
* `import tkinter as tk`
* `import sys, os, configparser, codecs`

## python Export%2520Timing%2520Points%2520to%2520TXT

Python tkinter example: Export%2520Timing%2520Points%2520to%2520TXT

```python
import sys, os, configparser, codecs
sys.argv=["Main"]

import tkinter as tk
from tkinter.filedialog import asksaveasfilename

root = tk.Tk()
root.withdraw()

proj = RPR_EnumProjects(-1, "", 512)[0]
(proj, project_dir, buf_sz) = RPR_GetProjectPathEx(proj, "", 255)

config_path = os.path.join(project_dir, "tempo_mapping.ini")
config = configparser.RawConfigParser()
config.read(config_path)

def update_txt_path():
  file_path = asksaveasfilename(initialdir=project_dir,defaultextension=".txt", filetypes=[("Text file",".txt")], title="Save Timing Points")
  if not file_path:
    RPR_ShowMessageBox("No file location provided!", "Export Error", 0)
    return False
  if not config.has_section('Export'):
    config.add_section('Export')
  config.set('Export', 'txt', file_path)
  with codecs.open(config_path, 'w', encoding='utf8') as f:
    config.write(f)
    return True

if not config.has_option('Export', 'txt'):
  update_txt_path()

def check_file_path():
  file_path = config.get('Export', 'txt')
  confirm =  RPR_ShowMessageBox("Export to: \n" + file_path, "Export Confirm", 3)
  if confirm == 7: # No
    if update_txt_path():
      export(config.get('Export', 'txt'))
  elif confirm == 6: # Yes
    export(config.get('Export', 'txt'))

def export(file_path):
  global proj
  f = open(file_path, 'w')

  tempo_c = RPR_CountTempoTimeSigMarkers(proj)
  last_timesig_num = 0
  last_timesig_denom = 0

  timing_points = []

  for ptidx in range(0, tempo_c):
    (ok, proj, ptidx, timepos, measurepos, beatpos, bpm, timesig_num, timesig_denom, lineartempo) = RPR_GetTempoTimeSigMarker(proj, ptidx, 0, 0, 0, 0, 0, 0, 0)
    if timesig_num == 0:
      timesig_num = last_timesig_num
    else:
      last_timesig_num = timesig_num
    if timesig_denom == 0:
      timesig_denom = last_timesig_denom
    else:
      last_timesig_denom = timesig_denom
    timing_points.append("%d,%s,%d,2,0,100,1,0\n" % (int(timepos * 1000), (60000 / bpm), timesig_denom))
  f.writelines(timing_points)

check_file_path()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
