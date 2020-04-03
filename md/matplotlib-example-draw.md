---
title: matplotlib example draw (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'draw'

Functions in program: 
* `def draw(dataset_name, alignment_names=[], slice=slice(None), drawing='tks', dim=2, notitle=False, open_pdf=False):`

Modules used in program: 
* `import matplotlib.gridspec as gridspec`
* `import numpy as np`

## python draw

Python matplotlib example: draw

```python
from pathlib import Path

import numpy as np

from matplotlib import pyplot as plt
from matplotlib.colors import LinearSegmentedColormap, NoNorm, to_rgba
from matplotlib.backends.backend_pdf import PdfPages
from matplotlib.collections import LineCollection
import matplotlib.gridspec as gridspec

def draw(dataset_name, alignment_names=[], slice=slice(None), drawing='tks', dim=2, notitle=False, open_pdf=False):
    eigen_files = sorted(eigen_npz(dataset_name))
    drawing_algorithm = globals()[drawing]
    edgelists = np.load(edgelists_pkl(dataset_name))

    d = working_dir.joinpath('drawing-' + drawing)
    d.mkdir(parents=True, exist_ok=True)

    black = to_rgba('black')

    print('  drawing:', end=' ', flush=True)
    for alignment_name in alignment_names:
        align = globals()[alignment_name]
        print(alignment_name, end=' ', flush=True)

        pdf_path = d.joinpath('{}-{}-{}.pdf'.format(
            dataset_name, str_slice(slice), alignment_name))

        i_last = -1
        eigen_last = None
        with PdfPages(str(pdf_path)) as pdf:
            for i, eigen_file in zip(range(len(eigen_files))[slice], eigen_files[slice]):
                eigen = np.load(eigen_file)
                eigen = eigen['eigenvalues'][0:50], eigen['eigenvectors'][:, 0:50]
                if eigen_last is not None: eigen = align(*eigen_last, *eigen)
                positions = drawing_algorithm(*eigen, dim=dim)

                X, Y = positions[:,0], positions[:,1]
                size = np.max(np.fabs(positions)) * 1.05

                fig = plt.figure(figsize=(5, 5))
                ax = plt.axes()

                plt.scatter(X, Y, marker='.', s=size * 0.02, c='black', linewidths=1)

                ax.add_collection(LineCollection([[(X[j], Y[j]), (X[k], Y[k])]
                                                  for j, k in edgelists[i]],
                                                 linewidths=[0.2],
                                                 colors=[to_rgba('black')]))

                ax.set_xbound(lower=-size, upper=size)
                ax.set_ybound(lower=-size, upper=size)

                if not notitle:
                    title = ('{}[{}] ({}, {})'.format(dataset_name, i, alignment_name, drawing)
                            if i_last == -1
                            else '{}[{}-{}] ({}, {})'.format(
                                dataset_name, i_last, i, alignment_name, drawing))
                    plt.title(title)

                pdf.savefig(fig, bbox_inches='tight')
                plt.close(fig)

                eigen_last, i_last = eigen, i

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
