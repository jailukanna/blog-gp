---
title: matplotlib example gen (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'gen'


Modules used in program: 
* `import matplotlib.delaunay as triang`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python gen

Python matplotlib example: gen

```python
# coding:utf-8

import numpy as np
import matplotlib.pyplot as plt
from skimage.segmentation import slic
from skimage.measure import regionprops
from matplotlib.patches import Polygon
from matplotlib.collections import PatchCollection
import matplotlib.image as mpimg
import matplotlib.delaunay as triang

IMG = 'foo.png'

fig, axes = plt.subplots(ncols=4, figsize=(12, 12))
ax0, ax1, ax2,ax3 = axes

# abrimos a imagem
image_rgb = mpimg.imread(IMG)

labels = slic(image_rgb,
              convert2lab=True,
              ratio=-10,
              n_segments=100,
              sigma=.1,
              max_iter=10)
conexos = labels
for ax in axes:
  ax.clear()

ax0.imshow(image_rgb, cmap=plt.cm.gray, interpolation='nearest')
ax1.imshow(conexos, cmap=plt.cm.jet, interpolation='nearest')
ax2.imshow(conexos, cmap=plt.cm.jet, interpolation='nearest')
ax3.imshow(image_rgb, cmap=plt.cm.gray, interpolation='nearest')
ax0.set_title('Original image', fontsize=10)
ax1.set_title('Segmentation', fontsize=10)
# calculamos pontos dos triânbulos através de Delaunay
cens, edg, tri, neig = triang.delaunay(pontos[:,0], pontos[:,1])
patches = []
cores = []
for t in tri:
  # t[0], t[1], t[2] are the points indexes of the triangle
  t_i = [t[0], t[1], t[2], t[0]]
  ax2.plot(pontos[:,0][t_i], pontos[:,1][t_i], 'k-', linewidth=.5)

  x = pontos[:,0][t_i][0]
  y = pontos[:,1][t_i][0]
  # a cor do polígono vem da imagem original            
  cores.append(image_rgb[y-1][x-1])
ax2.set_title('Delaunay triangulation', fontsize=10)
ax3.set_title('Generated image', fontsize=10)
                                    
# calculamos os boundingbox e centróides das regiões
# guardamos os pontos dos boundingbox em pontos
pontos = []
for region in regionprops(conexos, ['Area', 'BoundingBox', 'Centroid']):
    minr, minc, maxr, maxc = region['BoundingBox']

    x0 = region['Centroid'][1]
    y0 = region['Centroid'][0]
    pontos.append([minc, minr])
    pontos.append([maxc, minr])
    pontos.append([minc, maxr])
    pontos.append([maxc, maxr])

pontos = np.array(pontos)
    
  # criamos um polígono para representar o triângulo (aqui começa o desenho
  # que estamos sintetizando)
  poly = Polygon(pontos[:,[0,1]][t_i], True)
  patches.append(poly)

# adicionamos os polígonos a uma coleção de colagens
p = PatchCollection(patches)
print('patches:', len(patches), 'regions:', len(cores))
# definimos as cores conforme imagem original  e as bordas em transparente
p.set_facecolor(cores)
p.set_edgecolor(cores)
p.set_alpha(.8)
# plotamos o desenho sintetizado
ax3.add_collection(p)
for ax in axes:
  ax.axis('off')
fig.savefig('gen_' + IMG)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
