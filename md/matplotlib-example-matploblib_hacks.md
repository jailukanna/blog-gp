---
title: matplotlib example matploblib hacks (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matploblib hacks'

Functions in program: 
* `def patch_matplotlib():`

## python matploblib hacks

Python matplotlib example: matploblib hacks

```python
def patch_matplotlib():
    import numpy as np
    import time
    import matplotlib
    from matplotlib import cbook
    from mpl_toolkits.mplot3d import Axes3D
    from mpl_toolkits.mplot3d import art3d
    from mpl_toolkits.mplot3d import proj3d
    from mpl_toolkits.mplot3d.art3d import Poly3DCollection
    from matplotlib.collections import PolyCollection
    from matplotlib.axes import Axes
    from matplotlib import path as mpath
    from matplotlib import pyplot


    assert matplotlib.__version__ == '3.1.3'


    def perimeters_2x2(x):
      rows, cols = x.shape
      upper = np.repeat(x[:-1], 2, axis=1)[:, 1:-1].reshape(rows - 1, cols - 1, 2)
      lower = np.repeat(x[1:], 2, axis=1)[:, 1:-1].reshape(rows - 1, cols - 1, 2)[..., ::-1]
      return np.concatenate((upper, lower), axis=2).reshape(-1, 4)


    def Axes3D_plot_surface(self, X, Y, Z, *args, norm=None, vmin=None,
                      vmax=None, lightsource=None, **kwargs):
        had_data = self.has_data()

        assert Z.ndim == 2
        assert X.shape == Y.shape == Z.shape
        rows, cols = Z.shape

        assert 'facecolors' not in kwargs
        assert 'color' not in kwargs

        cmap = kwargs.get('cmap', None)

        for name in ['rstride', 'cstride', 'rcount', 'ccount']:
          kwargs.pop(name, None)

        polys = np.stack([perimeters_2x2(arr) for arr in (X, Y, Z)], axis=-1) # (num_surfaces, 4, 3)
        polyc = art3d.Poly3DCollection(polys, *args, **kwargs)
        
        avg_z = polys[..., 2].mean(axis=1)
        polyc.set_array(avg_z)
        # TODO: can those speed anything up?
        if vmin is not None or vmax is not None:
            polyc.set_clim(vmin, vmax)
        if norm is not None:
            polyc.set_norm(norm)

        self.add_collection(polyc)
        # TODO: can this be made faster?
        self.auto_scale_xyz(X, Y, Z, had_data)

        return polyc
    Axes3D.plot_surface = Axes3D_plot_surface


    def _proj_transform_vec(vec, M):
        vecw = np.dot(M, vec)
        w = vecw[3]
        # clip here..
        return vecw[:3] / w


    def Poly3DCollection_do_3d_projection(self, renderer):
        # print('3D projection!!')
        # s = time.perf_counter()
        # FIXME: This may no longer be needed?
        if self._A is not None:
            self.update_scalarmappable()
            self._facecolors3d = self._facecolors

        # print(1, time.perf_counter() - s)
        txs, tys, tzs = tvec = _proj_transform_vec(self._vec, renderer.M)
        num_faces = tvec.shape[1] // 4
        assert self._vec.shape == (4, num_faces * 4)
        assert tvec.shape == (3, num_faces * 4)
        assert tzs.shape == (num_faces * 4,)

        # print(2, time.perf_counter() - s)
        # This extra fuss is to re-order face / edge colors
        cface = self._facecolors3d
        cedge = self._edgecolors3d
        if len(cface) != num_faces:
            cface = cface.repeat(num_faces, axis=0)
        if len(cedge) != num_faces:
            if len(cedge) == 0:
                cedge = cface
            else:
                cedge = cedge.repeat(num_faces, axis=0)

        # print(3, time.perf_counter() - s)

        idx = np.argsort(self._zsortfunc(tzs.reshape(num_faces, 4), axis=1))[::-1]
        segments_2d = tvec[:2].reshape(2, num_faces, 4).transpose((1, 2, 0))[idx]

        # print(4, time.perf_counter() - s)
        
        if self._codes3d is not None:
            assert False, "Unoptimized path!"
            codes = [self._codes3d[idx] for z, s, fc, ec, idx in z_segments_2d]
            PolyCollection.set_verts_and_codes(self, segments_2d, codes)
        else:
            PolyCollection.set_verts(self, segments_2d, self._closed)

        # print(5, time.perf_counter() - s)

        assert len(cface) == len(idx)
        self._facecolors2d = cface[idx]
        if len(self._edgecolors3d) == len(cface):
            self._edgecolors2d = cedge[idx]
        else:
            self._edgecolors2d = self._edgecolors3d

        # print(6, time.perf_counter() - s)

        # Return zorder value
        if self._sort_zpos is not None:
            zvec = np.array([[0], [0], [self._sort_zpos], [1]])
            ztrans = proj3d._proj_transform_vec(zvec, renderer.M)
            return ztrans[2][0]
        elif tzs.size > 0:
            # FIXME: Some results still don't look quite right.
            #        In particular, examine contourf3d_demo2.py
            #        with az = -54 and elev = -45.
            return np.min(tzs)
        else:
            return np.nan
    Poly3DCollection.do_3d_projection = Poly3DCollection_do_3d_projection


    def Poly3DCollection_get_vector(self, segments3d): # (num_faces, 4, 3)
        num_segments = segments3d.shape[0]
        coords_first = segments3d.transpose((2, 0, 1)).reshape(3, -1)
        num_points = coords_first.shape[1]
        ones = np.ones((1, num_points))
        self._vec = np.concatenate((coords_first, ones), axis=0)

        segis = np.repeat(np.arange(0, num_points + 1, 4), 2)[1:-1].reshape(num_segments, 2)
        self._segis = segis
    Poly3DCollection.get_vector = Poly3DCollection_get_vector


    def Poly3DCollection_update_surface(self, X, Y, Z):
        polys = np.stack([perimeters_2x2(arr) for arr in (X, Y, Z)], axis=-1) # (num_surfaces, 4, 3)
        self.set_verts(polys)
        self.set_array(polys[..., 2].mean(axis=1))
    Poly3DCollection.update_surface = Poly3DCollection_update_surface


    def PolyCollection_set_verts(self, verts, closed=True):
        if closed:
            if len(verts) == 0:
              self._paths = []
            elif hasattr(self, '_cached_verts'):
              assert verts.shape == (self._cached_verts.shape[0], 4, 2)
              self._cached_verts[:, :4, :] = verts
              self._paths = self._cached_paths
            else:
              num_paths = verts.shape[0]
              assert verts.shape == (num_paths, 4, 2)
              self._cached_verts = verts = np.concatenate((verts, verts[:, -1:]), axis=1)
              codes = np.empty(5, dtype=mpath.Path.code_type)
              codes[:] = mpath.Path.LINETO
              codes[0] = mpath.Path.MOVETO
              codes[-1] = mpath.Path.CLOSEPOLY
              self._cached_paths = self._paths = [mpath.Path(xy, codes) for xy in verts]
        else:
            self._paths = [mpath.Path(xy) for xy in verts]
        self.stale = True
    PolyCollection.set_verts = PolyCollection_set_verts


    def pyplot_savefig(*args, **kwargs):
        assert 'transparent' not in kwargs
        return pyplot.gcf().savefig(*args, **kwargs)
    pyplot.savefig = pyplot_savefig

patch_matplotlib()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
