---
title: PIL example Import (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Import'

Functions in program: 
* `def main():`
* `def get_exif(fn):`

Modules used in program: 
* `import Plot`
* `import clipboard`
* `import Image`
* `import appex`

## python Import

Python PIL example: Import

```python
import appex
import Image
from PIL.ExifTags import TAGS
import clipboard
import Plot


def get_exif(fn):
	ret = {}
	i = Image.open(fn)
	info = i._getexif()
	for tag, value in info.items():
		decoded = TAGS.get(tag, tag)
		ret[decoded] = value
	return ret

def main():
	if not appex.is_running_extension():
		print('This script is intended to be run from the sharing extension.')
		return
	files = appex.get_file_paths()
	if files:
		with open(files[0], "r") as rfile_:
			data = rfile_.read()
			
		components = files[0].split("/")
		l = len(components)
		
		name = "Logs/" + components[l-1]
		
		print(name)
		
		with open(name, "w") as wfile_:
			wfile_.write(data)
			
	else:
		print('No input image found')

if __name__ == '__main__':
	main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
