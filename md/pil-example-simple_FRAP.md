---
title: pil example simple FRAP (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'simple FRAP'


Modules used in program: 
* `import matplotlib`
* `import omero`

## python simple FRAP

Python pil example: simple FRAP

```python

# coding: utf-8

# ### Please use the following link to try out the OMERO Python language bindings
# https://docs.openmicroscopy.org/latest/omero/developers/Python.html

# ### Import Packages required to connect to OMERO

# In[1]:


from omero.gateway import BlitzGateway

from getpass import getpass


# ### Create a connection to an OMERO server

# In[2]:


HOST = 'outreach.openmicroscopy.org'
PORT = 4064
conn = BlitzGateway(raw_input("Username: "), getpass("OMERO Password: "), host=HOST, port=PORT)
conn.connect()


# In[6]:


image_id = 28662
roi_service = conn.getRoiService()
result = roi_service.findByImage(image_id, None)
shape_id = None
for roi in result.rois:
    for s in roi.copyShapes():
        shape_id = s.id.val
        print(shape_id)


# In[7]:


the_c = 0
image = conn.getObject('Image', image_id)
size_t = image.getSizeT()
meanvalues = []
for t in range(size_t):
    stats = roi_service.getShapeStatsRestricted([shape_id], 0, t, [the_c])
    meanvalues.append(stats[0].mean[the_c])
print(meanvalues)


# In[9]:


import omero
key_value_data = [[str(t), str(meanvalues[t])] for t in range(size_t)]
map_ann = omero.gateway.MapAnnotationWrapper(conn)
namespace = "demo.simple_frap_data"
map_ann.setNs(namespace)
map_ann.setValue(key_value_data)
map_ann.save()
image.linkAnnotation(map_ann)


# In[10]:


from PIL import Image
import matplotlib
matplotlib.use('Agg')
from matplotlib import pyplot as plt
fig = plt.figure()
plt.subplot(111)
plt.plot(meanvalues)
fig.canvas.draw()
fig.savefig('plot.png')
pil_img = Image.open('plot.png')
pil_img.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
