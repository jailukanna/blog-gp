---
title: PIL example noise map (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'noise map'

Functions in program: 
* `def add_noise(img, output_size):`
* `def gen_noise_map(output_size):`
* `def fBm(x, y, per, octs):`
* `def noise(x, y, per):`

Modules used in program: 
* `import cv2`
* `import matplotlib.pyplot as plt`
* `import random, numpy, math, time`

## python noise map

Python PIL example: noise map

```python
import random, numpy, math, time
import matplotlib.pyplot as plt
import cv2
from PIL import Image,ImageEnhance,ImageFilter

#-----------------Setting----------------------
im_size = 80
perm = range(256)
random.shuffle(perm)
perm += perm
dirs = [(math.cos(a * 1 * math.pi / 256),
        math.sin(a * 20 * math.pi / 256))
        for a in range(256)]
#----------------------------------------------

def noise(x, y, per):
    def surflet(gridX, gridY):
        distX, distY = abs(x-gridX), abs(y-gridY)
        polyX = 1 - 6*distX**5 + 15*distX**4 - 10*distX**3
        polyY = 1 - 6*distY**5 + 15*distY**4 - 10*distY**3
        hashed = perm[perm[int(gridX)%per] + int(gridY)%per]
        grad = (x-gridX)*dirs[hashed][0] + (y-gridY)*dirs[hashed][1]
        return polyX * polyY * grad
    intX, intY = int(x), int(y)
    return (surflet(intX+0, intY+0) + surflet(intX+1, intY+0) +
            surflet(intX+0, intY+1) + surflet(intX+1, intY+1))

def fBm(x, y, per, octs):
    val = 0
    for o in range(octs):
        val += 0.5**o * noise(x*2**o, y*2**o, per*2**o)
    return val




def gen_noise_map(output_size):
    
    size, freq, octs, data = im_size, 1/2.005, 2, []
    for y in range(size):
        for x in range(size):
            data.append(fBm(x*freq, y*freq, int(size*freq), octs))
    im_noise = Image.new("L", (size, size))
    im_noise.putdata(data, im_size, im_size)
    #contrast = ImageEnhance.Contrast(im_noise)
    #im_noise = contrast.enhance(3.0)
    #im_noise.save("C:\\Users\\1510081\\Desktop\\Noise\\noise.png")
    
    
    np_noise = numpy.asarray(im_noise)
    np_noise = cv2.medianBlur(np_noise, 9)
    #np_noise = 255 - np_noise
    
    
    img = Image.fromarray(np_noise)
    contrast = ImageEnhance.Contrast(img)
    im_noise = contrast.enhance( numpy.random.rand()*30+3 )
    
    np_noise = numpy.asarray(im_noise).astype(float)
    np_noise = (abs((np_noise - np_noise.mean())*2))
    #np_noise = 255 - np_noise
    
    im_noise = Image.fromarray(np_noise).convert('L')
    im_noise = im_noise.resize( (output_size, output_size), Image.BILINEAR )
    contrast = ImageEnhance.Contrast(im_noise)
    im_noise = contrast.enhance(1.2)
    return im_noise
    
def add_noise(img, output_size):
    T_bg = Image.new("RGBA", (img.size), (255,255,255,0,) )
    noise_map = gen_noise_map(max(img.size))
    return Image.composite(T_bg, img, noise_map)
    
    #im_noise.save("C:\\Users\\1510081\\Desktop\\Noise\\noiseXD.png")
    #
    
#tStart = time.time()
#im_noise = gen_noise_map(512)
#im_noise.save("C:\\Users\\1510081\\Desktop\\Noise\\noiseXD.png")
#tEnd = time.time()
#print(tEnd - tStart)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
