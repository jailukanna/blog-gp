---
title: matplotlib example benchmark new auto legend locator (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'benchmark new auto legend locator'

Functions in program: 
* `def draw_polygon(prng, N_edge_max, extent, **kwargs):`
* `def draw_collection(coll, ax=None):`
* `def draw_patches(patch_list, ax=None):`
* `def draw_hist(data, ax=None, **kwargs):`
* `def _new_auto_legend_data(self):`
* `def _former_auto_legend_data(self):`

Modules used in program: 
* `import pprofile  # === pprofile version ===`
* `import mock  # to monkey-patch the relevant method of ``Legend```
* `import matplotlib.legend as mlegend`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python benchmark new auto legend locator

Python matplotlib example: benchmark new auto legend locator

```python
# -*- coding: utf-8 -*-
"""
Memo for pprofile:
pprofile --out cachegrind.out benchmark_cleverer_legend.py >> profiling.log
"""

from __future__ import division, print_function, unicode_literals

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.legend as mlegend
import mock  # to monkey-patch the relevant method of ``Legend``

import pprofile  # === pprofile version ===
#from line_profiler import LineProfiler  # === line_profiler version ===

from matplotlib.patches import Polygon
from matplotlib.collections import PatchCollection

# Import what is needed in ``_auto_legend_data``
from matplotlib.lines import Line2D
from matplotlib.patches import Rectangle, Patch

#%%
# The methods that one wants to profile


def _former_auto_legend_data(self):
    assert self.isaxes

    ax = self.parent
    bboxes = []
    lines = []
    offsets = []

    for handle in ax.lines:
        assert isinstance(handle, Line2D)
        path = handle.get_path()
        trans = handle.get_transform()
        tpath = trans.transform_path(path)
        lines.append(tpath)

    for handle in ax.patches:
        assert isinstance(handle, Patch)

        if isinstance(handle, Rectangle):
            transform = handle.get_data_transform()
            bboxes.append(handle.get_bbox().transformed(transform))
        else:
            transform = handle.get_transform()
            # The former strategy
            bboxes.append(handle.get_path().get_extents(transform))

    for handle in ax.collections:
        transform, transOffset, hoffsets, paths = handle._prepare_points()

        if len(hoffsets):
            for offset in transOffset.transform(hoffsets):
                offsets.append(offset)

    try:
        vertices = np.concatenate([l.vertices for l in lines])
    except ValueError:
        vertices = np.array([])

    return [vertices, bboxes, lines, offsets]


def _new_auto_legend_data(self):
    assert self.isaxes

    ax = self.parent
    bboxes = []
    lines = []
    offsets = []

    for handle in ax.lines:
        assert isinstance(handle, Line2D)
        path = handle.get_path()
        trans = handle.get_transform()
        tpath = trans.transform_path(path)
        lines.append(tpath)

    for handle in ax.patches:
        assert isinstance(handle, Patch)

        if isinstance(handle, Rectangle):
            transform = handle.get_data_transform()
            bboxes.append(handle.get_bbox().transformed(transform))
        else:
            transform = handle.get_transform()
            # The suggested new strategy
            tpath = transform.transform_path(handle.get_path())
            lines.append(tpath)

    for handle in ax.collections:
        transform, transOffset, hoffsets, paths = handle._prepare_points()

        if len(hoffsets):
            for offset in transOffset.transform(hoffsets):
                offsets.append(offset)

    try:
        vertices = np.concatenate([l.vertices for l in lines])
    except ValueError:
        vertices = np.array([])

    return [vertices, bboxes, lines, offsets]

#%%
# Helper functions for plotting

def draw_hist(data, ax=None, **kwargs):
    if ax is None:
        fig_label = "histogram"
        plt.close(fig_label)
        fig, ax = plt.subplots(num=fig_label)

    _bins = np.linspace(data.min() - 0.01, data.max() + 0.01, len(data)//10)
    kwargs.setdefault("bins", _bins)
    ax.hist(data, **kwargs)

    return ax


def draw_patches(patch_list, ax=None):
    if ax is None:
        fig_label = "patches"
        plt.close(fig_label)
        fig, ax = plt.subplots(num=fig_label)

    for _p in patch_list:
        ax.add_patch(_p)

    return ax


def draw_collection(coll, ax=None):
    if ax is None:
        fig_label = "collection"
        plt.close(fig_label)
        fig, ax = plt.subplots(num=fig_label)

    ax.add_collection(coll)

    return ax


def draw_polygon(prng, N_edge_max, extent, **kwargs):
    """Return a random Polygon with at most *N_edge_max* edges,
    all of them in the *extent* (xmin, xmax, ymin, ymax).
    """
    xmin, xmax, ymin, ymax = extent
    N_edges = prng.randint(3, N_edge_max)

    x_values = (xmax - xmin)*prng.random_sample(size=(N_edges, 1)) + xmin
    y_values = (ymax - ymin)*prng.random_sample(size=(N_edges, 1)) + ymin

    return Polygon(np.hstack((x_values, y_values)), **kwargs)

#%%
# It's plotting time!

N_iterations = 50  # Repeat the plots several times to average the profiling
N_hist_samples = 50000
N_patches = 500  # Use a multiple of 4 if one wants this exact amount
N_edge_max = 20  # >= 3 (please)

for mth, lbl in (
        (_former_auto_legend_data, "former"), (_new_auto_legend_data, "new")):

    with mock.patch.object(mlegend.Legend, "_auto_legend_data", mth):
        prng = np.random.RandomState(390423)  # same seed in both cases

        print("\n=== Benchmarking the {0} auto legend locator ===".format(lbl))

        # Prepare the figure and the relevant axes
        fig_label = "benchmark_of_{0}_method".format(lbl)
        plt.close(fig_label)
        fig, (ax0, ax1, ax2) = plt.subplots(
                                ncols=3, num=fig_label, figsize=(12.8, 4.8))

        # Set up a profiler instance for each type of plot
        # === line_profiler version ===
        #lp0 = LineProfiler()
        #ax0_lgd_wrapper = lp0(ax0.legend)
        #lp1 = LineProfiler()
        #ax1_lgd_wrapper = lp1(ax1.legend)
        #lp2 = LineProfiler()
        #ax2_lgd_wrapper = lp2(ax2.legend)
        # === pprofile version ===
        prof0 = pprofile.Profile()
        prof1 = pprofile.Profile()
        prof2 = pprofile.Profile()

        for idx in range(N_iterations):

            # Histogram with a high amount of bins
            data = prng.random_sample(size=N_hist_samples)
            data[data < 0.1] = 0.01
            data[data > 0.9] = 0.99
            ax = ax0
            ax.clear()
            ax = draw_hist(data, ax=ax, histtype="step", label="Data")

            # Axes with a significant amount of ``Patch`` (distinct) instances
            # NB: target a centered legend as it's the last possible location.
            patch_list = []
            _N4 = N_patches // 4
            for extent in ([0.0, 1.0, 0.8, 1.0], [0.0, 0.2, 0.0, 1.0],
                           [0.0, 1.0, 0.0, 0.2], [0.8, 1.0, 0.0, 1.0]):
                patch_list += [
                    draw_polygon(prng, N_edge_max, extent) for _ in range(_N4)]
            ax = ax1
            ax.clear()
            ax = draw_patches(patch_list, ax=ax)
            ax.plot([0.0, 1.0], [0.9, 0.9], color="tab:red", label="A segment")

            # Axes using a ``PatchCollection`` instance instead
            patch_coll = PatchCollection(patch_list, color="tab:blue")
            ax = ax2
            ax.clear()
            ax = draw_collection(patch_coll, ax=ax)
            ax.plot([0.0, 1.0], [0.9, 0.9], color="tab:red", label="A segment")

            # Update the canvas with the auto legend locator

            # === line_profiler version ===
            #for ax_legend_wrapper in (
            #        ax0_lgd_wrapper, ax1_lgd_wrapper, ax2_lgd_wrapper):
            #    ax_legend_wrapper(loc="best")
            # === pprofile version ===
            for prof, ax in ((prof0, ax0), (prof1, ax1), (prof2, ax2)):
                with prof():
                    ax.legend(loc="best")

            fig.canvas.draw()

            if (idx % 10) == 0:
                fig.savefig(fig.get_label() + "_{:03d}.png".format(idx))

        for plot_type, prof in (
                ("hist.", prof0), ("patch.", prof1), ("coll.", prof2)):

            print("\nProfile for the '{0}' plot:".format(plot_type))
            prof.print_stats()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
