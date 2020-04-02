---
title: pil example Image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Image'


Modules used in program: 
* `import simplejson as json`
* `import urllib2, uuid, os`
* `import socket`

## python Image

Python pil example: Image

```python
import socket
import urllib2, uuid, os
from PIL import Image, ImageChops, ImageOps
import simplejson as json
from settings import MEDIA_ROOT
from studio.misc import PhotoSize

"""

Dependencies: PIL
Settings: MEDIA_ROOT (directory where files are stored)

Following class is used in the class usage examples:

    class PhotoSize:
        Timeline=(940, 375)
        Thumbnail=(195, 146)
        CoverThumbnail = (275, 125)
        Thumbnail84x84 = (84, 84)
        Logo = (60, 60)

"""

class BaseImageProcessor:
    """
    utility to resize images
    """
    def __init__(self, base_dir, **kwargs):
        self.base_dir = base_dir
        self.id = uuid.uuid1()
        self.quality = 90
        self.extension = None
        self.success = True

    def save(self):
        pass

    def get_original(self):
        return '%s.%s' % (self.id, self.extension)

    def get_filename(self):
        return '%s/%s.%s' % (self.base_dir, self.id, self.extension)

    def get_extension(self, filename):
        return filename.split('/')[-1:][0].split('.')[-1:][0]

    def clean_up(self):
        os.remove(self.get_filename())

    def rescale(self, dimension, force=False):
        nw = dimension[0]
        nh = dimension[1]
        padding = False

        try:
            img = Image.open(self.get_filename())

            pw, ph = img.size
            pr = float(pw) / float(ph)
            nr = float(nw) / float(nh)

            if force:
                # this is mainly used for logos or images that shouldn't be cropped
                img.thumbnail((nw, nh), Image.ANTIALIAS)

            elif pr >= nr:
                # photo aspect is wider than destination ratio
                tw = int(round(nh * pr))

                if pw < tw:
                    padding = True
                else:
                    # reduce width
                    img = img.resize((tw, nh), Image.ANTIALIAS)
                    l = int(round(( tw - nw ) / 2.0))
                    img = img.crop((l, 0, l + nw, nh))

            elif pr < nr:
                # photo aspect is taller than destination ratio
                th = int(round(nw / pr))

                if ph < th:
                    padding = True
                else:
                    # reduce height
                    img = img.resize((nw, th), Image.ANTIALIAS)
                    t = int(round(( th - nh ) / 2.0))
                    img = img.crop((0, t, nw, t + nh))

            if padding:
                # add black padding instead of stretching
                img = img.crop( (0, 0, nw, nh) )
                offset_x = max(int(round(( nw - pw ) / 2.0)), 0)
                offset_y = max(int(round(( nh - ph ) / 2.0)), 0)
                img = ImageChops.offset(img, offset_x, offset_y)

            filename = '%s_%s_%s.%s' % (
                self.id,
                nw,
                nh,
                self.extension
            )

            img.save('%s/%s' % (self.base_dir, filename), 'JPEG', quality = self.quality)
            return filename

        except IOError:
            self.success = False

    def is_success(self):
        return self.success

    def get_json_response(self):
        result = {'success': self.is_success(),
                  'original': self.get_original(),
                  'id': '%s' % self.id}
        return json.dumps(result)


class URLImageProcessor(BaseImageProcessor):
    """
    utility to download images from the web, save them
    and resize them.

    usage:

    processor = URLImageProcessor(url='http://www.projectsherpa.com/static/img/companies/moat/logo.png')
    if processor.save():
        timeline = processor.rescale(PhotoSize.Timeline)
        thumbnail = processor.rescale(PhotoSize.Thumbnail, True) # adding True here forces a thumbnail without doing a resize
        ...
    """
    def __init__(self, base_dir=MEDIA_ROOT, **kwargs):
        BaseImageProcessor.__init__(self, base_dir, **kwargs)
        self.url = kwargs.pop('url')
        self.extension = self.get_extension(self.url)

    def save(self):
        if '?' in self.url:
            return False

        try:
            f = urllib2.urlopen(self.url)
            contents = f.read()

            local_file = open(self.get_filename(), 'wb')
            local_file.write(contents)
            local_file.close()
            return True
        except (urllib2.HTTPError, urllib2.URLError, socket.timeout, socket.error, ValueError) as e:
            print('Download Error', str(e))
            return False


class FileImageProcessor(BaseImageProcessor):
    """
    utility to resize image files

    usage:

    processor = FileImageProcessor(filename=filename)
    # uploaded is of type HttpRequest in this example
    # see: https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest.read
    processor.load(uploaded) 
    timeline = processor.rescale(PhotoSize.Timeline)
    thumbnail = processor.rescale(PhotoSize.Thumbnail, True)
    """

    def __init__(self, base_dir=MEDIA_ROOT, **kwargs):
        BaseImageProcessor.__init__(self, base_dir, **kwargs)
        self.filename = kwargs.pop('filename')
        self.extension = self.get_extension(self.filename)
        self.contents = None

    def save(self):
        try:
            local_file = open(self.get_filename(), 'wb')
            local_file.write(self.contents)
            local_file.close()
            return True
        except IOError:
            return False

    def load(self, contents):
        self.contents = contents.read()
        self.save()





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
