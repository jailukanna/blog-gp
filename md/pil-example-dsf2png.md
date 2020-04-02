---
title: pil example dsf2png (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dsf2png'


Modules used in program: 
* `import sys`
* `import PIL.Image, PIL.PngImagePlugin`
* `import numpy`

## python dsf2png

Python pil example: dsf2png

```python
#!/usr/bin/python
#
# Extracts the water v land from an X-Plane 10 "Global Scenery" or "HD Mesh Scenery v3" DSF
# and saves it as a 1bpp PNG image.
#
# Requires Python 2.7 plus the numpy and PIL modules, and 7-Zip
#

# Decompression requires `brew install p7zip` on Mac, 7-Zip command line on Windows, or p7zip on linux
P7 = '/usr/local/bin/7za'

# Requires X-Plane Command-Line tools - http://developer.x-plane.com/tools/xptools/
DSFTOOL = '~/Desktop/X-Plane 10/tools/DSFTool'

# Output folder
OUTDIR = '.'


import numpy
import PIL.Image, PIL.PngImagePlugin

from os import spawnl, P_WAIT, unlink
from os.path import basename, expanduser, isfile, join
import sys
from tempfile import gettempdir

if len(sys.argv) not in [2,3]:
    sys.exit('''Extracts the water v land from an X-Plane DSF and saves it as a 1bpp PNG image

Usage:	dsf2png DSF_file [PNG_file]
''')

infile = sys.platform == 'win32' and sys.argv[1] or expanduser(sys.argv[1])
if not isfile(infile):
    sys.exit('"%" doesn\'t exist!' % infile)
else:
    infile = sys.argv[1]

if len(sys.argv) == 2:
    outfile = join(OUTDIR, basename(sys.argv[1][:-4]) + '.png')
else:
    outfile = sys.argv[2]
if sys.platform != 'win32':
    outfile = expanduser(outfile)


if sys.platform == 'win32':
    e = spawnl(P_WAIT, P7, '"%s"' % P7, 'e', '"%s"' % infile, '-y', '-o%s' % gettempdir())
else:
    e = spawnl(P_WAIT, P7, P7, 'e', infile, '-y', '-o%s' % gettempdir())
if e:
    sys.exit('"%s" exited with %d!' % (P7, e))


if sys.platform == 'win32':
    e = spawnl(P_WAIT, DSFTOOL, '"%s"' % DSFTOOL, '--dsf2text', '"%s"' % join(gettempdir(), basename(infile)), '"%s"' % join(gettempdir(), basename(infile)[:-4] + '.txt'))
else:
    e = spawnl(P_WAIT, expanduser(DSFTOOL), DSFTOOL, '--dsf2text', join(gettempdir(), basename(infile)), join(gettempdir(), basename(infile)[:-4] + '.txt'))
if e:
    sys.exit('"%s" exited with %d!' % (DSFTOOL, e))
if not isfile(join(gettempdir(), basename(infile)[:-4] + '.txt.elevation.raw')):
    sys.exit('"%s" doesn\'t contain an elevation DEM - is this an X-Plane 10 Global Scenery DSF?' % infile)


# Source data is signed, but import as unsigned because we're just interested in non-zero
dem = numpy.fromfile(join(gettempdir(), basename(infile)[:-4] + '.txt.elevation.raw'), 'H')
assert len(dem) == 1201 * 1201
dem = numpy.flipud(dem.reshape((1201, 1201)))

# elevation DEM is post-centric. We need to convert to point-centric
dem = dem.clip(0, 1)	# only interested in whether non-zero elevation or not
post = dem[:-1,:-1] + dem[1:,:-1] + dem[:-1,1:] + dem[1:,1:]
post = post.astype('B')
assert post.shape == (1200, 1200)

# write as PNG
image = PIL.Image.frombuffer('L', post.shape, post, 'raw', 'L', 0, 1)
if True:	# 1bpp
    image = image.point(lambda x: x and 1 or 0, mode='1')
    image.save(outfile, format='PNG', optimize=True)
else:		# 8bpp, 5 levels
    image = image.point([0,63,127,191] + [255]*252)
    image.save(outfile, format='PNG', transparency=0, optimize=True)


# cleanup
unlink(join(gettempdir(), basename(infile)))
unlink(join(gettempdir(), basename(infile)[:-4] + '.txt'))
unlink(join(gettempdir(), basename(infile)[:-4] + '.txt.elevation.raw'))
unlink(join(gettempdir(), basename(infile)[:-4] + '.txt.sea_level.raw'))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
