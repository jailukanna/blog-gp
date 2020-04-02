---
title: PIL example image methods (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image methods'

Functions in program: 
* `def THUMB2(image, nx=120, ny=120):`
* `def THUMB(image, nx=120, ny=120):`

## python image methods

Python PIL example: image methods

```python
########################################################################
# IMAGE METHODS
########################################################################
class RESIZE(object): 
    def __init__(self,nx=160,ny=80,error_message=' image resize'): 
        (self.nx,self.ny,self.error_message)=(nx,ny,error_message) 
    def __call__(self,value):
        if isinstance(value, str) and len(value)==0: 
            return (value,None) 
        from PIL import Image 
        import cStringIO 
        try: 
            img = Image.open(value.file) 
            img.thumbnail((self.nx,self.ny), Image.ANTIALIAS) 
            s = cStringIO.StringIO() 
            img.save(s, 'JPEG', quality=100) 
            s.seek(0) 
            value.file = s 
        except: 
            return (value, self.error_message) 
        else: 
            return (value, None)
            
def THUMB(image, nx=120, ny=120):
	if image:
		from PIL import Image 
		import os  
		img = Image.open(request.folder + 'static/uploads/' + image)
		img.thumbnail((nx,ny), Image.ANTIALIAS) 
		root,ext = os.path.splitext(image)
		thumb='%s_thumb%s' %(root, ext)
		img.save(request.folder + 'static/uploads/' + thumb)
		return thumb
    
def THUMB2(image, nx=120, ny=120):
    from PIL import Image 
    import os  
    img = Image.open(request.folder + 'uploads/' + image)
    img.thumbnail((nx,ny), Image.ANTIALIAS) 
    root,ext = os.path.splitext(image)
    thumb='%s_thumb%s' %(root, ext)
    img.save(request.folder + 'uploads/' + thumb)
    return thumb


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
