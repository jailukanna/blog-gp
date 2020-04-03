---
title: matplotlib example textbbox (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'textbbox'

Functions in program: 
* `def draw_text_boxes(text, format, dpi):`
* `def hide_frame(axes):`

Modules used in program: 
* `import matplotlib`
* `import itertools`

## python textbbox

Python matplotlib example: textbbox

```python
mode = ['png', 'svg', 'pdf', 'pgf'][3]
dpi = 100

poem = '''
Farewell sweet earth and northern sky,
for ever blest, since here did lie,
and here with lissom limbs did run,
beneath the moon, beneath the sun,
L\xfathien Tin\xfaviel
more fair than mortal tongue can tell.
Though all to ruin fell the world,
and were dissolved and backward hurled
unmade into the old abyss,
yet were its making good, for this\u2014
the dawn, the dusk, the earth, the sea\u2014
that L\xfathien on a time should be!
'''.strip('\n').split('\n')

poem[2]  += r' $\int dx\,\ln x=\frac{1}{x}+C$'
poem[6]  += r' $e=\sum_{n=0}^\infty\frac{1}{n!}$'
poem[10] += r' $\tan x=\frac{\sin x}{\cos x}$'

import itertools

import matplotlib
if mode == 'pgf':
  matplotlib.use('pgf')
from matplotlib import pyplot

def hide_frame(axes):
  axes.set_frame_on(False)
  axes.tick_params(bottom=False, top=False, left=False, right=False,
    labelcolor='none')
  for tick in itertools.chain(
    axes.xaxis.get_major_ticks(),
    axes.xaxis.get_minor_ticks(),
    axes.yaxis.get_major_ticks(),
    axes.yaxis.get_minor_ticks()):
    tick.set_visible(False)

pyplot.xlabel('\n'.join(poem[8:12]),
  linespacing=1.5, rotation=-10, ha='right', rotation_mode='anchor')
pyplot.ylabel('\n'.join(poem[0:4]),
  linespacing=1.5, rotation=30)
pyplot.annotate('\n'.join(poem[4:8]), (0.15, 0.15),
  linespacing=1.5)
pyplot.subplots_adjust(left=0.35, bottom=0.35)
hide_frame(pyplot.gca())

def draw_text_boxes(text, format, dpi):
  figure = text.figure
  canvas = figure.canvas._get_output_canvas(format)

  figure.dpi, dpi = dpi, figure.dpi

  import io
  getattr(canvas, 'print_' + format)(io.BytesIO(), dryrun=True)
  renderer = figure._cachedRenderer

  from matplotlib.lines import Line2D
  from matplotlib.patches import Rectangle
  from matplotlib.transforms import Affine2D

  tx = text.convert_xunits(text._x)
  ty = text.convert_yunits(text._y)
  tx, ty = text.get_transform().transform((tx, ty))
  _, coords, _ = text._get_layout(renderer)
  coords = [(tx+x, ty+y, dx, dy) for _, (dx, dy), x, y in coords]

  depths = []
  for line in text.get_text().split('\n'):
    line, is_math = text.is_math_text(line, text.get_usetex())
    if line:
      _, _, d = renderer.get_text_width_height_descent(
        line, text.get_fontproperties(), is_math)
    else:
      d = 0
    depths.append(d)

  rotation = text.get_rotation()
  transforms = [
    Affine2D().rotate_deg(rotation).translate(x, y)
    for x, y, _, _ in coords]

  pyplot.gcf().lines += (
    Line2D((0, dx), (0, 0), transform=t, zorder=0)
    for (_, _, dx, _), t in zip(coords, transforms))
  pyplot.gcf().artists += (
    Rectangle((0, -d), dx, dy, transform=t, zorder=0, alpha=0.3)
    for (_, _, dx, dy), d, t in zip(coords, depths, transforms))

  figure.dpi = dpi

draw_text_boxes(pyplot.gca().xaxis.label, mode, dpi)
draw_text_boxes(pyplot.gca().yaxis.label, mode, dpi)
draw_text_boxes(pyplot.gca().texts[0],    mode, dpi)
pyplot.savefig('textbbox.' + ('pdf' if mode == 'pgf' else mode), dpi=dpi)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
