---
title: tkinter example Export%2520Timing%2520Points%2520to%2520OSU (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Export%2520Timing%2520Points%2520to%2520OSU'

Functions in program: 
* `def export(file_path):`
* `def timing_points_lines():`
* `def check_file_path():`
* `def update_osu_path():`

Modules used in program: 
* `import tkinter as tk`
* `import sys, os, configparser, codecs, re`

## python Export%2520Timing%2520Points%2520to%2520OSU

Python tkinter example: Export%2520Timing%2520Points%2520to%2520OSU

```python
import sys, os, configparser, codecs, re
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

def update_osu_path():
  file_path = asksaveasfilename(initialdir=project_dir,defaultextension=".osu", filetypes=[("osu! file",".osu")], title="Save Timing Points")
  if not file_path:
    RPR_ShowMessageBox("No file location provided!", "Export Error", 0)
    return False
  if not config.has_section('Export'):
    config.add_section('Export')
  config.set('Export', 'osu', file_path)
  with codecs.open(config_path, 'w', encoding='utf8') as f:
    config.write(f)
    return True

if not config.has_option('Export', 'osu'):
  update_osu_path()

def check_file_path():
  file_path = config.get('Export', 'osu')
  confirm =  RPR_ShowMessageBox("Export to osu: \n" + file_path, "Export Confirm", 3)
  if confirm == 7: # No
    if update_osu_path():
      export(config.get('Export', 'osu'))
  elif confirm == 6: # Yes
    export(config.get('Export', 'osu'))

def timing_points_lines():
  global proj
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

  return timing_points

def export(file_path):
  with codecs.open(file_path, 'r', encoding='utf8') as f:
    lines = f.readlines()
    idxs = [ i for i, line in enumerate(lines) if line.startswith('[TimingPoints]') ]
    if any(idxs):
      first_part = lines[:idxs[0]+1]
      second_part = lines[idxs[0]+1:]
      idxs = [ i for i, line in enumerate(second_part) if re.search("^\[\w*\]", line) ]
      if any(idxs):
        second_part = second_part[idxs[0]:]
        lines = first_part + timing_points_lines() + ['\r\n', '\r\n', '\r\n'] + second_part
        with codecs.open(file_path, 'w', encoding='utf8') as f:
          f.writelines(lines)

check_file_path()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
