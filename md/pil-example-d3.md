---
title: pil example d3 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'd3'

Functions in program: 
* `def dicom_ask_for_dir():`

Modules used in program: 
* `import pyqtgraph.console as pgcon`
* `import pyqtgraph.opengl as gl`
* `import pyqtgraph as pg`
* `import numpy as np`
* `import dicom `
* `import glob`
* `import os`

## python d3

Python pil example: d3

```python

"3-d visualization tools"

import os
import glob
import dicom 
import numpy as np
import pyqtgraph as pg
import pyqtgraph.opengl as gl
import pyqtgraph.console as pgcon

from pyqtgraph import QtCore, QtGui


def dicom_ask_for_dir():
    cfg = pg.QtCore.QSettings('INS', 'IC/DC')
    path = cfg.value('dicomdir')
    if type(path) not in (str, unicode):
        path = unicode(path.toString())
    print(path, type(path), len(path)==0)
    """
    if len(path) == 0:
        path = os.path.expanduser('~')
    """
    newpath = unicode(pg.QtGui.QFileDialog.getExistingDirectory(
        directory=path,
        caption='Select folder containing DICOM files'
    ))
    if len(newpath)>0:
        cfg.setValue('dicomdir', newpath)
        return newpath
    return ''

# refactor to separate module
class XImageViewBox(pg.ViewBox):

    sigPosChanged = pg.QtCore.Signal(object)

    def __init__(self, image=None):
        pg.ViewBox.__init__(self)
        self.image = image or np.random.randn(255, 255)
        self.item = pg.ImageItem()
        self.item.setImage(self.image)
        self.addItem(self.item)
        self.il_x = pg.InfiniteLine(pos=self.image.shape[0]/2, angle=90)
        self.il_y = pg.InfiniteLine(pos=self.image.shape[1]/2, angle=0)
        self.addItem(self.il_x)
        self.addItem(self.il_y)

    def tr_pixels_data(self, px, normed=False):
        x, y = px
        ((vxlo, vxhi), (vylo, vyhi)) = self.viewRange()
        r = self.rect()
        rxlo, rxhi, rylo, ryhi = r.left(), r.right(), r.bottom(), r.top()
        nx = (x - rxlo)/(rxhi - rxlo)
        ny = (y - rylo)/(ryhi - rylo)
        if normed:
            return nx, ny
        return nx*(vxhi - vxlo) + vxlo, ny*(vyhi - vylo) + vylo

    def mouseClickEvent(self, ev):
        pos = ev.x(), ev.y()
        dx, dy = self.tr_pixels_data(pos)
        self.il_x.setValue(dx)
        self.il_y.setValue(dy)
        self.sigPosChanged.emit(self.tr_pixels_data(pos, normed=True))

    # disable other interactions
    def mouseDragEvent(self, ev):
        pass

    def wheelEvent(self, ev):
        pass

class XImageWidget(pg.GraphicsView):

    def __init__(self, image=None):
        pg.GraphicsView.__init__(self)
        self.xi = XImageViewBox(image=image)
        self.setCentralItem(self.xi)

    # mouse dragging continously sets position

    _pressed = False
    def mousePressEvent(self, ev):
        self._pressed = True
        self.xi.mouseClickEvent(ev)

    def mouseReleaseEvent(self, ev):
        self._pressed = False

    def mouseMoveEvent(self, ev):
        if self._pressed:
            self.xi.mouseClickEvent(ev)

class Slice(XImageWidget):

    sigXChanged = pg.QtCore.Signal(object)
    sigYChanged = pg.QtCore.Signal(object)

    def __init__(self, axis, data, T=False, fx=False, fy=False):
        XImageWidget.__init__(self)
        self.axis = axis
        self.data = data
        self.T = T
        self.fx = fx
        self.fy = fy
        self.shape = list(self.data.shape)
        del self.shape[axis]
        self.xi.sigPosChanged.connect(self.pos_changed)
        self.ignore_pos_changes = False
        self.idx = data.shape[axis]/2

    def pos_changed(self, pos):
        x, y = pos
        # ignore changes when calling set_pos
        if not self.ignore_pos_changes:
            self.sigXChanged.emit(x)
            self.sigYChanged.emit(y)

    def set_pos(self, x=None, y=None):
        self.ignore_pos_changes = True
        xa, ya = (1, 0) if self.T else (0, 1)
        if x:
            self.xi.il_x.setValue(x * self.shape[xa])
        if y:
            self.xi.il_y.setValue(y * self.shape[ya])
        self.ignore_pos_changes = False

    _idx = 0

    def _get_idx(self):
        return self._idx

    def _set_idx(self, idx):
        self._idx = idx = int(idx)
        self.update_image()

    def update_image(self):
        sl = [slice(None) for i in range(self.axis)] + [self.idx]
        try:
            ary = self.data[tuple(sl)]
        except Exception as exc:
            print('no slice', exc, sl)
            return
        jx = -1 if self.fx else 1
        jy = -1 if self.fy else 1
        self.xi.item.setImage((ary.T if self.T else ary)[::jx, ::jy])

    idx = property(_get_idx, _set_idx)

    _slice = 0.0

    def _get_slice(self):
        return self._slice

    def _set_slice(self, val):
        self.idx = val * self.data.shape[self.axis]

    slice = property(_get_slice, _set_slice)


class ImageContrastControl():
    pass


class SlicesVolume(gl.GLViewWidget):

    def __init__(self, images, datasets=None, parent=None):
        gl.GLViewWidget.__init__(self, parent=parent)
        self.images = np.transpose(images, (2, 1, 0)).copy()
        self.datasets = datasets
        self.volume = np.zeros(self.images.shape + (4,), np.ubyte)

        self.images  = (self.images - self.images.min())*255.0/self.images.ptp()

        print(self.images.min(), self.images.max())
    
        p10, p90 = np.percentile(self.images.flat[:], [1, 99])
        self.images = np.clip((self.images - p10)/(p90-p10)*256, 0, 255).astype(np.ubyte)
        self.volume[..., 0] = self.images
        self.volume[..., 1] = self.images
        self.volume[..., 2] = self.images
        #self.volume[..., 1] = 255 - self.images
        #self.volume[..., 2] = 255 - 2*np.abs(self.images - 128)
        self.volume[..., 3] = self.images/10

        self.item = gl.GLVolumeItem(self.volume)
        self.item.scale(*[1.0/s for s in self.images.shape])
        self.item.translate(-0.5, -0.5, -0.5)
        self.addItem(self.item)

    def keyPressEvent(self, ev):
        k = str(ev.text())
        print(k, k.lower(), k.lower() in 's')
        if k and k.lower() in 'xcv':
            tr = [0, 0, 0]
            tr['xcv'.index(k.lower())] = 10*(-1 if k==k.lower() else 1)
            self.item.translate(*tr)
        if k and k.lower() in 'sdf':
            sc = [1, 1, 1]
            sc['sdf'.index(k.lower())] = 1 + (0.2 if k==k.lower() else -0.2)
            self.item.scale(*sc)
        if k and k.lower() in 'Aa':
            self.item.data[..., -1] *= 0.5 if k=='a' else 2
            self.item.initializeGL()
            self.item.scale(1,1,1)


class FlipBoard(pg.QtGui.QWidget):
    def __init__(self, slices):
        pg.QtGui.QWidget.__init__(self)
        self.lay = pg.QtGui.QGridLayout()
        self.setLayout(self.lay)
        self.refs = []
        for i, ax in enumerate('xyz'):
            for j, attr in enumerate('T fx fy'.split()):
                cb = pg.QtGui.QCheckBox(ax + ' ' + attr)
                def _(obj, attr):
                    def __(state):
                        setattr(obj, attr, state>0)
                        obj.update_image()
                    return __
                ij = _(slices[i], attr)
                cb.stateChanged.connect(ij)
                self.lay.addWidget(cb, j, i)
                self.refs.append((cb, ij))


class Controls(pg.QtGui.QWidget):
    def __init__(self, datasets):
        pg.QtGui.QWidget.__init__(self)
        self.datasets = datasets
        self.lay = pg.QtGui.QGridLayout()
        self.setLayout(self.lay)
        self.pbs = []
        for k in dir(self):
            if k.startswith('pb_'):
                f = getattr(self, k)
                k = k.split('_')
                pb = pg.QtGui.QPushButton(' '.join(k[3:]).title())
                pb.clicked.connect(f)
                self.lay.addWidget(pb, int(k[1]), int(k[2]))
                self.pbs.append(pb)

    dups = []
    def pb_0_0_duplicate(self):
        self.dups.append(MultiXImage.from_datasets(self.datasets))
        self.dups[-1].show()

    lastpath = [os.path.expanduser('~')]
    def pb_1_0_open(self):
        path = dicom_ask_for_dir()
        if path:
            self.newmxi = MultiXImage.from_glob(os.path.join(path, '*'))
            self.newmxi.show()

    def pb_0_1_show_volume(self):
        image = np.array([d.pixel_array for d in self.datasets])
        self.volwin = SlicesVolume(image, self.datasets)
        self.volwin.show()


    def pb_0_2_metadata(self):
        self.metadatatw = pg.TableWidget()
        ds = self.datasets[0]
        self.metadata = []
        for k in ds.dir():
            v = ds.data_element(k)
            if v=='PixelData' or v is None:
                continue
            self.metadata.append((k, repr(v.value).encode('ascii', 'ignore')[:20]))
        self.metadatatw.setData(np.array(self.metadata, dtype=object))
        self.metadatatw.show()


class MultiXImage(pg.QtGui.QWidget):

    sigXChanged, sigYChanged, sigZChanged = [pg.QtCore.Signal(object) for _ in range(3)]

    def __init__(self, image, datasets=None, parent=None):
        pg.QtGui.QWidget.__init__(self, parent=parent)
        self.lay = pg.QtGui.QGridLayout()
        self.setLayout(self.lay)
        self.image = image
        self.setup_image()
        self.setup_signals()
        self.datasets = datasets
        if datasets is not None:
            self.ctls = Controls(datasets)
            self.lay.addWidget(self.ctls)

    def setup_image(self):

        im = self.image

        self.sl_x = Slice(0, im, fy=False)
        self.sl_y = Slice(1, im)
        self.sl_z = Slice(2, im, T=True)

        self.lay.addWidget(self.sl_x, 0, 0)
        self.lay.addWidget(self.sl_y, 0, 1)
        self.lay.addWidget(self.sl_z, 1, 0)
    
    def change_x(self, x):
        self.sl_y.slice = x
        self.sl_z.set_pos(x=x)
        self.sl_x.set_pos(x=x)
        self.sigXChanged.emit(x)

    def change_y(self, y):
        self.sl_x.slice = y
        self.sl_z.set_pos(y=y)
        self.sl_y.set_pos(x=y)
        self.sigYChanged.emit(y)

    def change_z(self, z):
        self.sl_z.slice = z
        self.sl_x.set_pos(y=z)
        self.sl_y.set_pos(y=z)
        self.sigZChanged.emit(z)

    def setup_signals(self):

        # connect x-y changes of slices to x-y-z slots
        self.sl_x.sigXChanged.connect(self.change_x)
        self.sl_x.sigYChanged.connect(self.change_z)

        self.sl_y.sigXChanged.connect(self.change_y)
        self.sl_y.sigYChanged.connect(self.change_z)

        self.sl_z.sigXChanged.connect(self.change_x)
        self.sl_z.sigYChanged.connect(self.change_y)


    @classmethod
    def from_glob(cls, pathglob):
        return cls.from_files(sorted(glob.glob(pathglob)))

    @classmethod
    def from_files(cls, files):
        return cls.from_datasets(map(dicom.read_file, files))

    @classmethod
    def from_datasets(cls, datasets):
        return cls(np.array([d.pixel_array for d in datasets]), datasets)

    @classmethod
    def from_file_dialog(cls):
        path = dicom_ask_for_dir()
        if path:
            return cls.from_glob(os.path.join(path, '*'))

if __name__ == '__main__':
    app = QtGui.QApplication([])
    mxi = MultiXImage.from_file_dialog()
    mxi.show()
    app.exec_()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
