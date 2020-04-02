---
title: PIL example ImgDownloader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ImgDownloader'

Functions in program: 
* `def imgDownloader(self, url, filename):`

## python ImgDownloader

Python PIL example: ImgDownloader

```python
def imgDownloader(self, url, filename):
        name_replce = filename.replace(' ', '-')
        fileext = os.path.splitext(url)[1]
        full_name = name_replce+fileext
        if not url:
            return ""
        try:
            filepath = os.path.join("img",full_name)
            if not os.path.exists("img"):
                os.makedirs("img")
            urllib.urlretrieve(url,filepath)
            if fileext in ('.jpg','.jpeg','.gif','.png','.svg'):
                full_name = full_name
            
        except Exception as e:
            # print("fetch {} error:{}".format(url,e),sys.stderr)
            full_name = ""

        if full_name:
            import PIL
            from PIL import Image
            path = os.getcwd()+"/img/"+full_name
            basewidth = 320
            img = Image.open(path)
            wpercent = (basewidth / float(img.size[0]))
            hsize = int((float(img.size[1]) * float(wpercent)))
            img = img.resize((basewidth, hsize), PIL.Image.ANTIALIAS)
            img.save(os.getcwd()+"/img/"+name_replce+'-thumb'+fileext)
            thumb = name_replce+'-thumb'+fileext
        else:
            thumb = ""

        return full_name, thumb

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
