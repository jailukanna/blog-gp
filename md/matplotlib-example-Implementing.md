---
title: matplotlib example Implementing (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Implementing'


Modules used in program: 
* `import matplotlib `
* `import matplotlib.pyplot as plt `

## python Implementing

Python matplotlib example: Implementing

```python
import matplotlib.pyplot as plt 
import matplotlib 
from sklearn.datasets import make_moons 
from sklearn.decomposition import KernelPCA
from sklearn.decomposition import PCA 

# Creating non-linear dataset
X, y = make_moons(n_samples = 2000, noise = 0.02, random_state = 1000) 

# Visualizing on this non-linear data
fig, (ax1, ax2, ax3) = plt.subplots(1,3)
colors = ['green','red']
ax1.scatter(X[:, 0], X[:, 1],c=y, cmap = matplotlib.colors.ListedColormap(colors)) 
ax1.set_title("Original space of Non-Linear Data")
ax1.set_xlabel("Feature 1") 
ax1.set_ylabel("Feature 2")

# Implementing PCA on this non-linear data
pca = PCA(n_components = 2) 
X_pca = pca.fit_transform(X) 

# Plot projections of PCA
ax2.set_title("Projection by PCA") 
ax2.scatter(X_pca[:, 0], X_pca[:, 1],c = y , cmap = matplotlib.colors.ListedColormap(colors)) 
ax2.set_xlabel("Principal Component 1") 
ax2.set_ylabel("Principal Component 2")

# Implement Kernel PCA 
kpca = KernelPCA(kernel ='rbf', gamma = 15) 
X_kpca = kpca.fit_transform(X) 

# Plot projections of Kernel PCA
ax3.set_title("Projection by Kernel PCA") 
ax3.scatter(X_kpca[:, 0], X_kpca[:, 1],c=y, cmap = matplotlib.colors.ListedColormap(colors))
ax3.set_xlabel("PC1 in space induced by kernel function") 
ax3.set_ylabel("Principal Component 2")
fig.set_figwidth(15)
plt.subplots_adjust(wspace=0.4)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
