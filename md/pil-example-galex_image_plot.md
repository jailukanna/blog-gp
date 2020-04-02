---
title: pil example galex image plot (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'galex image plot'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import sys`
* `import requests`
* `import PIL`
* `import xml.etree.ElementTree as ET`

## python galex image plot

Python pil example: galex image plot

```python
import xml.etree.ElementTree as ET

import PIL
import requests
from StringIO import StringIO

import sys
from PIL import Image
import matplotlib.pyplot as plt
from astropy.io import fits
from astropy.wcs import WCS

ra, dec = sys.argv[1], sys.argv[2]

r = requests.request('GET', 'http://galex.stsci.edu/gxWS/SIAP/gxSIAP.aspx?POS=%s,%s&SIZE=0.1' % (ra, dec))
data = ET.XML(r.content)
resource = data.find('{http://www.ivoa.net/xml/VOTable/v1.1}RESOURCE')
table = resource.find('{http://www.ivoa.net/xml/VOTable/v1.1}TABLE').find(
    '{http://www.ivoa.net/xml/VOTable/v1.1}DATA').find('{http://www.ivoa.net/xml/VOTable/v1.1}TABLEDATA')

img_dict = dict()
for child in table.getchildren():
    # print(child[14].text, child[20].text)
    img_dict[child[0].text+'_'+child[14].text] = child[20].text

i_fig = 1
for survey in ("AIS", "MIS"):
    img2 = Image.open(StringIO(requests.request('GET', img_dict['%s_FUV+NUV' % survey]).content))
    w = WCS(fits.getheader(img_dict["%s_NUV" % survey]))
    pos = w.wcs_world2pix(float(ra), float(dec), 0)
    plt.figure(i_fig)
    plt.clf()
    plt.imshow(img2.transpose(PIL.Image.FLIP_TOP_BOTTOM).crop((pos[0] - 67, pos[1] - 67, pos[0] + 67, pos[1] + 67)))
    plt.title(survey)
    i_fig +=1


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
