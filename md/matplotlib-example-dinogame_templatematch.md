---
title: matplotlib example dinogame templatematch (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'dinogame templatematch'

Functions in program: 
* `def update(i):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import glob`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import imageio`

## python dinogame templatematch

Python matplotlib example: dinogame templatematch

```python
import imageio
import numpy as np
import matplotlib.pyplot as plt
from skimage.color import rgb2gray
from skimage.feature import match_template
from matplotlib.gridspec import GridSpec
import glob
import matplotlib.animation as animation
from IPython.display import HTML
plt.rcParams['font.family'] = 'sans-serif'

#mp4 to images
filename = 'dino_game1.mp4'
vid = imageio.get_reader(filename, 'ffmpeg')
imgs=[]
for image in vid.iter_data():
    imgs.append(image)

len(imgs)
#299

metadata = vid.get_meta_data()
vid.close()
metadata
'''
{'plugin': 'ffmpeg',
 'nframes': inf,
 'ffmpeg_version': '4.2 built with gcc 7.3.0 (crosstool-NG 1.23.0.449-a04d0)',
 'codec': 'h264',
 'pix_fmt': 'yuv420p(tv',
 'fps': 60.0,
 'source_size': (854, 480),
 'size': (854, 480),
 'duration': 4.98}
'''

imgs = np.array(imgs)

#show image index=0
image =  rgb2gray(imgs[0])
plt.imshow(image,cmap='gray')
#<matplotlib.image.AxesImage at 0x7b83bef85b10>

#template image
sabo = image[310:370,505:590]
plt.imshow(sabo,cmap='gray')
#<matplotlib.image.AxesImage at 0x7b83bea320d0>

#match_template
result = match_template(image, sabo,pad_input=True)
ij = np.unravel_index(np.argmax(result), result.shape)
x, y = ij[::-1]

fig = plt.figure(figsize=(6, 4))
gs = GridSpec(2, 3)
ax1 = fig.add_subplot(gs[:, 0])
ax2 = fig.add_subplot(gs[0, 1:])
ax3 = fig.add_subplot(gs[1, 1:], sharex=ax2, sharey=ax2)

ax1.imshow(sabo, cmap=plt.cm.gray)
ax1.set_axis_off()
ax1.set_title('template')

ax2.imshow(image, cmap=plt.cm.gray)
ax2.set_axis_off()
ax2.set_title('image')
# highlight matched region
hsabo, wsabo = sabo.shape
rect = plt.Rectangle((x-wsabo/2, y-hsabo/2), wsabo, hsabo, edgecolor='green', facecolor='none')
ax2.add_patch(rect)

ax3.imshow(result,cmap='Spectral_r')
ax3.set_axis_off()
ax3.set_title('`match_template`\nresult')
# highlight matched region
#ax3.autoscale(False)
ax3.plot(x, y, 'o', markeredgecolor='green', markerfacecolor='none', markersize=10)
plt.savefig("sabo_match_2.png",dpi=150)
plt.show()

imgs = [rgb2gray(imgs[i]) for i in range(len(imgs))]
xs=[]
ys=[]
max_v = []
for i in range(len(imgs)):
    result = match_template(imgs[i], sabo,pad_input=True)
    max_v.append(result.max())
    if result.max() > 0.9:
        ij = np.unravel_index(np.argmax(result), result.shape)
        x, y = ij[::-1]
        xs.append(x)
        ys.append(y)
    else:
        xs.append(np.nan)
        ys.append(np.nan)        

#show result max
plt.plot(max_v)
plt.savefig('max_v.png',dpi=130)

#Animation
fig, ax = plt.subplots()
ax.imshow(imgs[0], cmap=plt.cm.gray)
hsabo, wsabo = sabo.shape
rect = plt.Rectangle((xs[0]-wsabo/2, ys[0]-hsabo/2), wsabo, hsabo, edgecolor='green', facecolor='none')
ax.add_patch(rect)
ax.axis('off')
imgss = []
def update(i):
    if len(imgss)>0:
        imgss.pop().remove()
        
    img = ax.imshow(imgs[i],cmap=plt.cm.gray)
    imgss.append(img)
    
    rect.set_xy((xs[i]-wsabo/2, ys[i]-hsabo/2))
    return fig,

ani = animation.FuncAnimation(fig, update, 299,interval=17, blit=True)
ani.save('dg_tmpmatch_anim.mp4', writer="ffmpeg",dpi=100)
HTML(ani.to_html5_video())

 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
