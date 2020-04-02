---
title: PIL example i3bg3 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'i3bg3'

Functions in program: 
* `def workspace_event(i3, evt):`
* `def gen_bg(pixmap, name):`
* `def change_workspace(name):`

Modules used in program: 
* `import random`
* `import os.path`
* `import sys`
* `import Xlib.display`
* `import Xlib`
* `import Xlib.threaded`

## python i3bg3

Python PIL example: i3bg3

```python
#!/usr/bin/env python3
#i3 per-workspace background for Caagr98 https://gist.github.com/Caagr98/f74e149403e5c1f92006158f09108a78
#deps: pip3 install python3-xlib i3ipc Pillow
# usage: i3bg2.py /path/to/directory
# where directory holds wallpapers to randomly pick from (optional)
import Xlib.threaded
import Xlib
import Xlib.display
from Xlib import Xatom
from PIL import Image
import sys
import os.path
import random
from collections import defaultdict
try:
	import i3ipc.i3ipc as i3ipc
except ImportError:
	import i3ipc

Image.Image.tostring = Image.Image.tobytes

if len(sys.argv) > 1:
	picture_path = sys.argv[1]
else:
	picture_path = ""

wallpaper_files = {}

#TODO: get workspaces with loaded_config_file_name() parsing the config?
workspace = {
	"1": "1: ",
	"2": "2: ",
	"3": "3: ",
	"4": "4: ",
	"5": "5: ",
	"6": "6: ",
	"7": "7: ",
	"8": "8: ",
	"9": "9: ♪",
	"10": "10: "
}
if os.path.isdir(picture_path):
	for x in range(1,11):
		wallpaper_files[workspace[str(x)]] = picture_path + os.sep + random.choice([x for x in os.listdir(os.path.expanduser(picture_path)) \
		if os.path.isfile(os.path.join(picture_path, x))])
else:
	# specify manually because no valid path submited
	wallpaper_files = {
	workspace['1']: "#0E0E0E",
	workspace['2']: "~/Pictures/wallpapers/f01e4257412885.59d4e4cd5e722.jpg",
	workspace['3']: "#0E0E0E",
	workspace['4']: "#0E0E0E",
	workspace['5']: "#0E0E0E",
	workspace['6']: "~/Pictures/wallpapers/MnmI88n.jpg",
	workspace['7']: "~/Pictures/wallpapers/37088990882_a324d208d4_o.png",
	workspace['8']: "~/Pictures/wallpapers/s2lj7bI.jpg",
	workspace['9']: "~/Pictures/wallpapers/MnmI88n.jpg",
	workspace['10']: "~/Pictures/test.jpg"
	}

backgrounds = defaultdict(lambda: "#0E0E0E", {
	workspace['1']: wallpaper_files[workspace['1']],
	workspace['2']: wallpaper_files[workspace['2']],
	workspace['3']: wallpaper_files[workspace['3']],
	workspace['4']: wallpaper_files[workspace['4']],
	workspace['5']: wallpaper_files[workspace['5']],
	workspace['6']: wallpaper_files[workspace['6']],
	workspace['7']: wallpaper_files[workspace['7']],
	workspace['8']: wallpaper_files[workspace['8']],
	workspace['9']:  wallpaper_files[workspace['9']],
	workspace['10']: wallpaper_files[workspace['10']]
})

i3ipc.WorkspaceEvent = lambda data, conn: data
i3ipc.GenericEvent = lambda data: data
i3ipc.WindowEvent = lambda data, conn: data
i3ipc.BarconfigUpdateEvent = lambda data: data
i3ipc.BindingEvent = lambda data: data
i3ipc.Con = lambda data, parent, conn: data
i3 = i3ipc.Connection()

background_cache = {}


def change_workspace(name):
	display = Xlib.display.Display()
	screen = display.screen()
	root = screen.root
	w, h = screen.width_in_pixels, screen.height_in_pixels

	if (name, w, h) not in background_cache:
		background_cache[name, w, h] = gen_bg(root.create_pixmap(w, h, screen.root_depth), name)

	id = background_cache[name, w, h].id
	root.change_property(display.get_atom("_XROOTPMAP_ID"), Xatom.PIXMAP, 32, [id])
	root.change_property(display.get_atom("ESETROOT_PMAP_ID"), Xatom.PIXMAP, 32, [id])
	root.change_attributes(background_pixmap=id)
	root.clear_area()
	display.sync()

def gen_bg(pixmap, name):
	geom = pixmap.get_geometry()
	w, h = geom.width, geom.height
	paint = pixmap.create_gc()
	bg = backgrounds[name]
	if bg[:1] == '#':
		paint.change(foreground=int(bg[1:], 16))
		pixmap.fill_rectangle(paint, 0, 0, w, h)
	else:
		im = Image.open(os.path.expanduser(bg))
		size = im.size
		if size[0] != w or size[1] != h:
			im = im.resize((w, h), Image.LANCZOS)
		try:
			pixmap.put_pil_image(paint, 0, 0, im)
		except Exception as e:
			print("Xlib error in drawable.put_pil_image():", e)

	return pixmap

for output in i3.get_outputs():
	if output["current_workspace"]:
		change_workspace(output["current_workspace"])

def workspace_event(i3, evt):
	if evt["change"] != "focus":
		return
	change_workspace(evt["current"]["name"])

i3.on("workspace", workspace_event)
i3.subscriptions = 0xFF
i3.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
