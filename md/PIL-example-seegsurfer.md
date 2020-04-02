---
title: PIL example seegsurfer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'seegsurfer'

Functions in program: 
* `def toggle_axis_visibility():`
* `def b_rot_deg_clicked(*_):`
* `def make_gif():`
* `def show_values():`
* `def toggle_labels():`
* `def toggle_spin():`
* `def do_spin():`
* `def take_shot():`
* `def create_balls(impl, log=True):`
* `def setup_hemispheres(basename, post='white', **glopt):`

Modules used in program: 
* `import ei`
* `import pyqtgraph.opengl as gl`
* `import pyqtgraph as pg`
* `import numpy as np`
* `import traceback`
* `import sys`
* `import os`

## python seegsurfer

Python PIL example: seegsurfer

```python
import os
import sys
import traceback

import numpy as np
import pyqtgraph as pg
import pyqtgraph.opengl as gl

# sidestep path mishandling
here = os.path.dirname(os.path.abspath(__file__))
sys.path.append(os.path.sep.join([here, 'deps']))

from nibabel.gifti import giftiio

import ei

app = pg.mkQApp()

class GiiSurface(object):
    def __init__(self, filename):
        self.obj = giftiio.read(filename)
        self.vert, self.face = [a.data for a in self.obj.darrays]
        self.face = self.face[:, [1,0,2]]
        self.mesh_data = gl.MeshData(vertexes=self.vert, faces=self.face)
    @property
    def center(self):
        return self.vert.mean(axis=0)

def setup_hemispheres(basename, post='white', **glopt):
    L = GiiSurface(basename + '_L%s.gii' % post)
    R = GiiSurface(basename + '_R%s.gii' % post)
    O = (L.center + R.center)/2
    all_vert = np.r_[L.vert, R.vert]
    all_vert -= all_vert.mean(axis=0)
    _, _, vt = np.linalg.svd(all_vert, full_matrices=False)
    rot_deg_horiz = np.arctan2(vt[0, 2], vt[0, 1]) / (2.0 * np.pi) * 360.0
    return O, rot_deg_horiz, L, R

def create_balls(impl, log=True):
    ball_items = []
    eis = np.concatenate([e['ei'] for e in impl.values()])
    eimin, eimax = np.percentile(eis, [5, 95])
    for name, elec in impl.items():
        ncont = len(elec.contacts)
        elei = (elec['ei'] - eimin)/eimax
        # normal direction from target to entry
        u = elec.entry - elec.target
        u /= np.linalg.norm(u)
        # contacts are 2mm long, 1.5mm spaced
        dr = 2.0 + 1.5
        r = np.array([c.index for c in elec.contacts]) * dr
        # handle bipolar / monopolar correctly
        bip = [c.bipolar for c in elec.contacts]
        if all(bip):
            r += dr/2.0
        elif any(bip):
            msg = 'mixed mono & bipolar electrodes not implemented'
            raise NotImplementedError(msg)
        # positions
        pos = elec.target + u*r[:, np.newaxis]
        # colors 
        color = np.zeros((ncont, 4))
        color[:, 0] = elei*0.8
        color[:, 1] = 0.2
        color[:, 2] = (1.0 - elei)*0.8
        color[:, 3] = 1.0
        # sizes
        size = 2 + (elei).astype(int)
        # add item
        ball_items.append(gl.GLScatterPlotItem(
            pos=pos, color=color, size=size, pxMode=False, glOptions='additive'))
        ball_items.append(gl.GLScatterPlotItem(
            pos=pos, color=color, size=size, pxMode=False, glOptions='translucent'))
    return ball_items

class GLView(gl.GLViewWidget):
    def __init__(self, *args, **kwds):
        gl.GLViewWidget.__init__(self, *args, **kwds)
        self.texts = []
    def paintGL(self):
        super(GLView, self).paintGL()
        white = pg.QtGui.QColor(255, 255, 255)
        black = pg.QtGui.QColor(0, 0, 0)
        for x, y, z, text, font in self.texts:
            self.qglColor(black)
            self.renderText(x+0.5, y+0.5, z+0.5, text, font)
            self.qglColor(black)
            self.renderText(x-0.5, y-0.5, z-0.5, text, font)
            self.qglColor(white)
            self.renderText(x, y, z, text, font)

"""
Immediate

- anywave plugin
- charge auto tout les fichiers
- colorbar pour valuers

ensuite

- matrix connectivity
- courbes + fleches (symm-no arrow, nonsymm nonzeros two)

- recalage des electrodes, 3xIRM
- click en 3D, repere 2d

- film tournant (export to avi,mov,gif?)
- rendu volume localization

donnees

- surfaces brain visa
- measure par contact (EI)
- position (target, entry)
- matrix d'couplage
- volume localization
- t1

- dynamique valeurs contacte, 
- topographie mais isocontour

"""


if True or __name__ == '__main__':
    last_arg = sys.argv[-1]
    if last_arg.endswith('gii'):
        giifile = last_arg
    else:
        opener = pg.QtGui.QFileDialog.getOpenFileName
        giifile = opener(parent=None, 
                         caption='Open left hemisphere surface',
                         filter='Gifti surfaces (*.gii)')
    if type(giifile) in (tuple,) and len(giifile) == 2:
        giifile, _ = giifile
    if giifile:
        parts = map(str, giifile.split('.gii')[0].split('_'))
        basename = '_'.join(parts[:-1])
        giitype = parts[-1][1:] # first letter is L or R, we don't care
    else:
        print('no file selected by user?')
        sys.exit(1)

basename, giitype = 'Cou_Pa', 'white'

try:
    O, rot_deg_horiz, L, R = setup_hemispheres(basename, post=giitype)
    impl = ei.parse_ei_txt(basename + '_ei.txt')
    ei.parse_locations(impl, basename + '_seeg.txt')
except Exception as exc:
    _, _, tb = sys.exc_info()
    msg = "Unable to read data files:\n\n%r" % (exc,)
    msg += '\nTraceback:\n\n'
    msg += '\n\n'.join(traceback.format_tb(tb))
    print(msg)
    w_msg = pg.QtGui.QErrorMessage()
    w_msg.showMessage(msg)
    app.exec_()
    sys.exit()

locballs = []
try:
    # QString doesn't have same methods as Python string
    py_str_fname = str(basename + '_loc.txt')
    locs = np.loadtxt(py_str_fname)
    if locs.ndim == 1:
        locs = locs.reshape((1, -1))
    for x, y, z, r, g, b, sz in locs:
        locballs.append(gl.GLScatterPlotItem(
            pos=np.r_[x,y,z], color=np.r_[r,g,b], size=sz))
except Exception as exc:
    import sys, traceback
    _, _, tb = sys.exc_info()
    traceback.print_tb(tb)
    print(exc)


# prep axes items
ax_coords = np.zeros((3, 2, 3))
ax_coords.flat[[3, 10, 17]] = 200.0
axis_line_items = [
    gl.GLLinePlotItem(
        pos=ax_coord, 
        color=np.r_[ax_coord[1], 1.0],
        width=10.0
        )
    for ax_coord in ax_coords]
glopt = {'shader': 'shaded',
         'glOptions': 'opaque',
         'smooth': True,
         'color': (0.7, 0.7, 0.7, 1.0)}
mL, mR = [gl.GLMeshItem(meshdata=S.mesh_data, **glopt) for S in (L, R)]
# main widget
win = pg.QtGui.QWidget()
win.resize(600, 600)
lay = pg.QtGui.QVBoxLayout()
win.setLayout(lay)
# visualization 
gvw = GLView()
lay.addWidget(gvw)
gvw.setCameraPosition(distance=200)
for item in [mL, mR] + create_balls(impl) + locballs + axis_line_items:
    item.translate(*(-O))
    item.rotate(rot_deg_horiz, 1.0, 0.0, 0.0)
    gvw.addItem(item)
# button container
lay_ctrl = pg.QtGui.QHBoxLayout()
lay.addLayout(lay_ctrl)
# screen shot
def take_shot():
    f = pg.QtGui.QFileDialog.getSaveFileName
    path, _ = f(caption='Save screenshot (PNG Image)', 
                filter='PNG Image (*.png)')
    gvw.grabFrameBuffer().save(path)
b_capt = pg.QtGui.QPushButton('Screenshot')
b_capt.clicked.connect(take_shot)
lay_ctrl.addWidget(b_capt)
# spin 
is_spinning = [False]
def do_spin():
    if is_spinning[0]:
        gvw.orbit(2, 0)
spin_timer = pg.QtCore.QTimer()
spin_timer.setInterval(10)
spin_timer.timeout.connect(do_spin)
spin_timer.start()
def toggle_spin():
    if is_spinning[0]:
        is_spinning[0] = False
        b_spin.setText('Spin')
    else:
        is_spinning[0] = True
        b_spin.setText('Stop Spin')
b_spin = pg.QtGui.QPushButton('Spin')
b_spin.clicked.connect(toggle_spin)
lay_ctrl.addWidget(b_spin)
# labels
def toggle_labels():
    if gvw.texts:
        del gvw.texts[:]
        b_labels.setText('Show labels')
    else:
        b_labels.setText('Remove labels')
        font = pg.QtGui.QFont()
        font.setWeight(75)
        font.setPointSize(18)
        for elec in impl.electrodes:
            vec = pg.QtGui.QVector3D(*elec.entry)
            v = mL.mapToView(vec)
            gvw.texts.append((v.x(), v.y(), v.z(), elec.region, font))
b_labels = pg.QtGui.QPushButton('Show labels')
lay_ctrl.addWidget(b_labels)
b_labels.clicked.connect(toggle_labels)
# value table
value_table = []
def show_values():
    eis = []
    for electrode in impl.electrodes:
        for contact in electrode.contacts:
            eis.append((contact.label, contact.ei))
    tw = pg.TableWidget()
    tw.setData(eis)
    tw.show()
    value_table.append(tw)
b_value_table = pg.QtGui.QPushButton('Table of values')
lay_ctrl.addWidget(b_value_table)
b_value_table.clicked.connect(show_values)
# gif 
def make_gif():
    f = pg.QtGui.QFileDialog.getSaveFileName
    path = f(caption='Save movie (GIF Animation)', 
             filter='GIF Image (*.gif)')
    try:
        path, _ = path
    except:
        pass
    try:
        import gif
        import cStringIO
        import PIL
    except Exception as exc:
        msg = pg.QtGui.QErrorMessage(parent=win)
        msg.showMessage("Unable to load libraries:\n\n%r" % (exc,))
        msg.wait()
        return None
    images = []
    pd = pg.QtGui.QProgressDialog(parent=win)
    pd.setModal(True)
    pd.setLabelText("Generating movie...")
    pd.show()
    pd.setMaximum(2*360)
    is_canceled = [False]
    def cancel():
        is_canceled[0] = True
    pd.canceled.connect(cancel)
    for i in range(2*360):
        if is_canceled[0]:
            break
        img = gvw.grabFrameBuffer()
        buff = pg.QtCore.QBuffer()
        buff.open(pg.QtCore.QIODevice.ReadWrite)
        img.save(buff, "PNG")
        sio = cStringIO.StringIO()
        sio.write(buff.data())
        buff.close()
        sio.seek(0)
        #images.append(PIL.Image.open(sio))
        ary_im = np.array(PIL.Image.open(sio))
        print(ary_im.shape)
        if ary_im.shape[0] % 2:
            ary_im = ary_im[:-1]
        if ary_im.shape[1] % 2:
            ary_im = ary_im[:, :-1]
        PIL.Image.fromarray(ary_im).save('shot%03d.png' % (i,))
        gvw.orbit(0.5, 0)
        pd.setValue(i)
        app.processEvents()
    pd.close()
b_gif = pg.QtGui.QPushButton('Make GIF')
lay_ctrl.addWidget(b_gif)
b_gif.clicked.connect(make_gif)
# add control to rotate around x axis
b_rot_deg = pg.QtGui.QPushButton('Rotate degrees:')
sb_rot_deg = pg.QtGui.QDoubleSpinBox()
sb_rot_deg.setMaximum(360.0)
sb_rot_deg.setMinimum(-360.0)
lay_ctrl.addWidget(b_rot_deg)
lay_ctrl.addWidget(sb_rot_deg)
def b_rot_deg_clicked(*_):
    deg = sb_rot_deg.value()
    for item in gvw.items:
        item.rotate(deg, 1.0, 0.0, 0.0)
b_rot_deg.clicked.connect(b_rot_deg_clicked)
# allow user to on/off the axes
def toggle_axis_visibility():
    flip = not axis_line_items[0].visible()
    for item in axis_line_items:
        item.setVisible(flip)
b_toggle_axis_visibility = pg.QtGui.QPushButton('Toggle axes')
b_toggle_axis_visibility.clicked.connect(toggle_axis_visibility)
lay_ctrl.addWidget(b_toggle_axis_visibility)
toggle_axis_visibility()
win.show()

if __name__ == '__main__':
    app.exec_()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
