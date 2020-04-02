---
title: PIL example invertImage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'invertImage'


Modules used in program: 
* `import PIL.ImageOps`
* `import omero.gateway`
* `import omero`

## python invertImage

Python PIL example: invertImage

```python

# Example of how to subclass ImageWrapper to extend renderImage()
# First part of this can be placed in a different python file
# As long as it is imported before being used
import omero
import omero.gateway
import PIL.ImageOps


class NewImageWrapper (omero.gateway.ImageWrapper):

    # def renderImage(self, z, t, compression=0.9):
    def renderImage(self, *args, **kwargs):
        invert = 'invert' in kwargs and kwargs['invert']
        if 'invert' in kwargs:
            del kwargs['invert']
        img = super(NewImageWrapper, self).renderImage(*args, **kwargs)
        if invert:
            img = PIL.ImageOps.invert(img)
        return img

# This makes sure that conn.getObject("Image") will return
# Our NewImageWrapper subclass
omero.gateway.ImageWrapper = NewImageWrapper
omero.gateway.refreshWrappers()


# Test it, possibly in a separate file
from omero.gateway import BlitzGateway
conn = BlitzGateway('will', 'ome')
conn.connect()

image = conn.getObject("Image", 3733)
print(image     # <NewImageWrapper id=3732>)

# without inverting
pilImg = image.renderImage(0, 0)
pilImg.show()

# with inverting
invertedImg = image.renderImage(0, 0, invert=True)
invertedImg.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
