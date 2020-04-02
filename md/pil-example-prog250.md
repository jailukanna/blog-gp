---
title: pil example prog250 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'prog250'


Modules used in program: 
* `import hashlib`

## python prog250

Python pil example: prog250

```python
#!/usr/bin/python
# Author: Kitwipat Towattana (@icheernoom)
 
import hashlib
from PIL import Image
 
md5 = []
width = 300 
height = 300
for i in range(0,10):
	img_file = "pixels{0}.png".format(i)
	img = Image.open(img_file)
	rgb_img = img.convert('RGB')
	xr = 0
	xg = 0
	xb = 0
	for x in range(0,width):
		for y in range(0,height):
			r, g, b = rgb_img.getpixel((x,y))
			xr += r
			xg += g
			xb += b
	r = hashlib.md5("{0}".format(xr)).hexdigest()
	g = hashlib.md5("{0}".format(xg)).hexdigest()
	b = hashlib.md5("{0}".format(xb)).hexdigest()
	sum_md5 = "{0}{1}{2}".format(r,g,b)
	concate = hashlib.md5("{0}".format(sum_md5)).hexdigest()
	print("[*] MD5 of {0}: {1}".format(img_file,concate))
	md5.append(concate)
print("[*] Flag:",hashlib.md5("".join(md5)).hexdigest())
 
'''
root@ubuntu:/PIL# ls
pixels0.png  pixels2.png  pixels4.png  pixels6.png  pixels8.png  prog250.py
pixels1.png  pixels3.png  pixels5.png  pixels7.png  pixels9.png  Thumbs.db
root@ubuntu:/PIL# python prog250.py 
[*] MD5 of pixels0.png: e767124634834f12a7152104d4713074
[*] MD5 of pixels1.png: a88372d92bcc6e8f5f569bc3c00fab23
[*] MD5 of pixels2.png: 345265539e1b9078323b7051346892de
[*] MD5 of pixels3.png: 92ae219f8b8403e04b550eb831a017bf
[*] MD5 of pixels4.png: a4bd4eb96fe9cc779c2e81864c81b674
[*] MD5 of pixels5.png: 4723f098c933b160d00678b7be3421c4
[*] MD5 of pixels6.png: 3a4c3cb7b2fd704c4e7717547f7db4d9
[*] MD5 of pixels7.png: bc1f5bc7b30eaac677d6daa37eed5e4c
[*] MD5 of pixels8.png: a4edf81b7f2915c4c1b72d8367c5016a
[*] MD5 of pixels9.png: 864ae043e67a693d7672986487a87813
[*] Flag: 2d98c27f040ce429b35dd84124397f65
root@ubuntu:/PIL# 
'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
