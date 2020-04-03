---
title: matplotlib example rotation (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'rotation'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import time`
* `import numpy as np`
* `import nibabel as nib`

## python rotation

Python matplotlib example: rotation

```python
from nfft.nfft import Plan
import nibabel as nib
import numpy as np
import time

# Load image
img = nib.load("./merge_cartesian_2D_0cycles.nii.gz").get_data()
width = img.shape[1]

# Zeropadding
zeropadding = width // 2
img = np.pad(img, zeropadding, mode='constant', constant_values=0)
width *= 2

# Sampling grid (defined to be in range -0.5 <= x < 0.5)
nodes = np.linspace(-0.5, 0.5, width, endpoint=False)
x, y = np.meshgrid(nodes, nodes)

# 2D rotation matrix: rotate by 45 degree
angle = np.pi / 4.0
rotMat = [[np.cos(angle), np.sin(angle)], [-np.sin(angle), np.cos(angle)]]

# Rotate sampling grid
knots = zip(np.ravel(x), np.ravel(y))
rotatedKnots = np.array([np.dot(rotMat, knot) for knot in knots])

# Forward transform
plan = Plan(img.shape, rotatedKnots.shape[0])
np.copyto(plan.f_hat, img)
np.copyto(plan.x, knots)
plan.precompute()
start = time.time()
plan.forward()
# plan stores the result of the forward transform in plan.f

# Adjoint
np.copyto(plan.x, rotatedKnots)
plan.precompute()
reconstruction = np.copy(plan.adjoint())
end = time.time()
print("Time elapsed: {:.2f}s".format((end - start)))

# Crop to original field of view
img = img[zeropadding:-zeropadding, zeropadding:-zeropadding]
reconstruction = np.abs(reconstruction)[zeropadding:-zeropadding, zeropadding:-zeropadding]

# Plot
import matplotlib
matplotlib.rcParams['backend'] = 'PDF'
matplotlib.rc('text', usetex=True)
matplotlib.rc('font', size=8)
import matplotlib.pyplot as plt
from matplotlib.cm import get_cmap

fig, axes = plt.subplots(1, 2, figsize=(6, 4), dpi=300)
axes[0].imshow(img, cmap=get_cmap('gray'))
axes[0].set_title('Original')
axes[0].axis('off')
axes[1].imshow(reconstruction, cmap=get_cmap('gray'))
axes[1].set_title('Rotated')
axes[1].axis('off')
fig.subplots_adjust(left=0.05, bottom=0.05, right=0.95, top=0.95, hspace=0.05, wspace=0.1)
fig.savefig('./rotation.pdf', format='pdf')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
