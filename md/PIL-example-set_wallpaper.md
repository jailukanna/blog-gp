---
title: PIL example set wallpaper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'set wallpaper'

Functions in program: 
* `def main(subreddit='EarthPorn', dirname=os.path.expanduser("~/.background"),`
* `def _set_wallpaper_mac(filename, color=(0, 0, 0)):`
* `def _set_wallpaper_gnome(filename, color=(0, 0, 0)):`
* `def set_wallpaper(filename, color=(0, 0, 0)):`
* `def average_color(filename, edge=.1):`
* `def pil_to_ndarray(im, dtype=None, img_num=None):`
* `def _palette_is_grayscale(pil_image):`
* `def download_a_link(subreddit, dirname, n=1):`

Modules used in program: 
* `import requests`
* `import praw`
* `import numpy as np`
* `import sys`
* `import subprocess`
* `import shutil`
* `import platform`
* `import os`
* `import codecs`

## python set wallpaper

Python PIL example: set wallpaper

```python
#!/usr/bin/python
'''
Script to set the OSX or Unity desktop background to the top picture from a
given subreddit.

NOTE: on OSX, sets it for the current space (on all monitors), but not other
spaces. There doesn't seem to be an AppKit API to change it for all spaces.
'''
from __future__ import division, print_function, unicode_literals

import codecs
import os
import platform
import shutil
import subprocess
import sys
from urlparse import urlparse

import numpy as np
import praw
import requests

system = platform.system()
if system not in {'Darwin', 'Linux'}:
    raise ValueError("Unknown system {}".format(system))

if system == 'Darwin':
    import AppKit
    import Foundation


# TODO: multireddits
def download_a_link(subreddit, dirname, n=1):
    r = praw.Reddit(user_agent='my_cool_background_changer')
    subs = r.subreddit(subreddit).hot(limit=10)

    for sub in subs:
        ext = sub.url.rsplit('.', 1)[-1][-6:]
        if ext.lower() not in {'jpg', 'jpeg', 'png'}:
            domain = urlparse(sub.url).netloc
            if domain not in {'i.reddituploads.com'}:
                continue
        if n != 1:
            n -= 1
            continue

        with codecs.open(os.path.join(dirname, 'meta'), 'w', encoding='utf-8') \
                as outfile:
            outfile.write(sub.title)
            outfile.write('\n')
            outfile.write(sub.url)
            outfile.write('\n')
            outfile.write('http://reddit.com' + sub.permalink)
            outfile.write('\n')

        resp = requests.get(sub.url, stream=True)
        img_fname = os.path.join(dirname, 'background.' + ext)

        with open(img_fname, 'wb') as outfile:
            shutil.copyfileobj(resp.raw, outfile)
        return img_fname

    raise TypeError("no images found")


def _palette_is_grayscale(pil_image):
    # skimage.io._plugins.pil_plugin.pil_to_ndarray
    # (modified BSD license)
    """Return True if PIL image in palette mode is grayscale.
    Parameters
    ----------
    pil_image : PIL image
        PIL Image that is in Palette mode.
    Returns
    -------
    is_grayscale : bool
        True if all colors in image palette are gray.
    """
    assert pil_image.mode == 'P'
    # get palette as an array with R, G, B columns
    palette = np.asarray(pil_image.getpalette()).reshape((256, 3))
    # Not all palette colors are used; unused colors have junk values.
    start, stop = pil_image.getextrema()
    valid_palette = palette[start:stop]
    # Image is grayscale if channel differences (R - G and G - B)
    # are all zero.
    return np.allclose(np.diff(valid_palette), 0)

def pil_to_ndarray(im, dtype=None, img_num=None):
    # skimage.io._plugins.pil_plugin.pil_to_ndarray
    # (modified BSD license)
    try:
        # this will raise an IOError if the file is not readable
        im.getdata()[0]
    except IOError as e:
        site = "http://pillow.readthedocs.org/en/latest/installation.html#external-libraries"
        pillow_error_message = str(e)
        error_message = ('Could not load "%s" \n'
                         'Reason: "%s"\n'
                         'Please see documentation at: %s'
                         % (im.filename, pillow_error_message, site))
        raise ValueError(error_message)
    frames = []
    grayscale = None
    i = 0
    while 1:
        try:
            im.seek(i)
        except EOFError:
            break

        frame = im

        if img_num is not None and img_num != i:
            im.getdata()[0]
            i += 1
            continue

        if im.format == 'PNG' and im.mode == 'I' and dtype is None:
            dtype = 'uint16'

        if im.mode == 'P':
            if grayscale is None:
                grayscale = _palette_is_grayscale(im)

            if grayscale:
                frame = im.convert('L')
            else:
                if im.format == 'PNG' and 'transparency' in im.info:
                    frame = im.convert('RGBA')
                else:
                    frame = im.convert('RGB')

        elif im.mode == '1':
            frame = im.convert('L')

        elif 'A' in im.mode:
            frame = im.convert('RGBA')

        elif im.mode == 'CMYK':
            frame = im.convert('RGB')

        if im.mode.startswith('I;16'):
            shape = im.size
            dtype = '>u2' if im.mode.endswith('B') else '<u2'
            if 'S' in im.mode:
                dtype = dtype.replace('u', 'i')
            frame = np.fromstring(frame.tobytes(), dtype)
            frame.shape = shape[::-1]

        else:
            frame = np.array(frame, dtype=dtype)

        frames.append(frame)
        i += 1

        if img_num is not None:
            break

        if hasattr(im, 'fp') and im.fp:
            im.fp.close()

        if img_num is None and len(frames) > 1:
            return np.array(frames)
        elif frames:
            return frames[0]
        elif img_num:
            raise IndexError('Could not find image  #%s' % img_num)


def average_color(filename, edge=.1):
    from PIL import Image

    im = pil_to_ndarray(Image.open(filename))
    h, w, c = im.shape
    assert c == 3
    dh = int(h * edge)
    dw = int(w * edge)
    parts = [im[dh:], im[-dh:], im[dh:-dh, :dw], im[dh:-dh, -dw:]]
    together = np.hstack([np.rollaxis(x, 2, 0).reshape(3, -1) for x in parts])
    return together.mean(axis=1)


def set_wallpaper(filename, color=(0, 0, 0)):
    fn = {
        'Darwin': _set_wallpaper_mac,
        'Linux': _set_wallpaper_gnome,
    }
    return fn[system](filename, color)


def _set_wallpaper_gnome(filename, color=(0, 0, 0)):
    def f(*args):
        return subprocess.check_call(
            ('gsettings', 'set', 'org.gnome.desktop.background') + args)
    f('picture-uri', 'file://{}'.format(os.path.abspath(filename)))
    f('picture-options', 'zoom')
    f('primary-color', '#{:02x}{:02x}{:02x}'.format(*(int(c) for c in color)))
    f('color-shading-type', 'solid')


def _set_wallpaper_mac(filename, color=(0, 0, 0)):
    file_url = Foundation.NSURL.fileURLWithPath_(filename)
    r, g, b = color
    color = AppKit.NSColor.colorWithCalibratedRed_green_blue_alpha_(
            int(r / 255), int(g / 255), int(b / 255), 1)
    options = {
        AppKit.NSWorkspaceDesktopImageScalingKey: AppKit.NSImageScaleProportionallyUpOrDown,
        AppKit.NSWorkspaceDesktopImageAllowClippingKey: AppKit.NO,
        AppKit.NSWorkspaceDesktopImageFillColorKey: color,
    }
    ws = AppKit.NSWorkspace.sharedWorkspace()
    for screen in AppKit.NSScreen.screens():
        result, error = ws.setDesktopImageURL_forScreen_options_error_(
            file_url, screen, options, None)
        if result is not True or error is not None:
            from IPython import embed; embed()
            print(result, error)
    subprocess.check_call(['killall', 'Dock'])


def main(subreddit='EarthPorn', dirname=os.path.expanduser("~/.background"),
         fill_color='average', n=1):
    if os.path.exists(dirname):
        shutil.rmtree(dirname)
    os.mkdir(dirname)

    img_file = download_a_link(subreddit, dirname, n=n)
    if fill_color == 'average':
        color = average_color(img_file)
    else:
        color = fill_color

    print("setting background to {}\n".format(img_file), file=sys.stderr)
    with codecs.open(os.path.join(dirname, 'meta'), 'r') as f:
        print(f.read().strip(), file=sys.stderr)
    set_wallpaper(img_file, color=color)


if __name__ == '__main__':
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('--dirname', '-d', default=os.path.expanduser('~/.background'))
    parser.add_argument('subreddit', default='EarthPorn', nargs='?')
    parser.add_argument('-n', type=int, default=1)
    g = parser.add_argument_group('fill color')
    g = g.add_mutually_exclusive_group()
    for name, val in [('white', (1, 1, 1)),
                      ('black', (0, 0, 0)),
                      ('fill-average', 'average')]:
        g.add_argument('--{}'.format(name), const=val,
                       action='store_const', dest='fill_color')
    parser.set_defaults(fill_color=(0, 0, 0))
    args = parser.parse_args()
    main(**vars(args))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
