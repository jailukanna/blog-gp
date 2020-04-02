---
title: PIL example face count and color (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'face count and color'

Functions in program: 
* `def main(tag, access_key):`
* `def detect_color_image(file=None, pil_img=None, thumb_size=40, MSE_cutoff=22, adjust_color_bias=True):`

Modules used in program: 
* `import hydrus`
* `import face_recognition as fr  # type: ignore`
* `import click`
* `import animeface as af  # type: ignore`
* `import logging`

## python face count and color

Python PIL example: face count and color

```python
#!/usr/bin/env python3
from tempfile import NamedTemporaryFile
from typing import List
import logging

from PIL import Image, ImageStat  # type: ignore
from tqdm import tqdm  # type: ignore
import animeface as af  # type: ignore
import click
import face_recognition as fr  # type: ignore
import hydrus


def detect_color_image(file=None, pil_img=None, thumb_size=40, MSE_cutoff=22, adjust_color_bias=True):
    assert any([file, pil_img]), 'file or PIL Image required'
    if file:
        pil_img = Image.open(file)
    bands = pil_img.getbands()
    res = {}
    if bands == ('R','G','B') or bands== ('R','G','B','A'):
        thumb = pil_img.resize((thumb_size,thumb_size))
        SSE, bias = 0, [0,0,0]
        if adjust_color_bias:
            bias = ImageStat.Stat(thumb).mean[:3]
            bias = [b - sum(bias)/3 for b in bias ]
        for pixel in thumb.getdata():
            mu = sum(pixel)/3
            SSE += sum((pixel[i] - mu - bias[i])*(pixel[i] - mu - bias[i]) for i in [0,1,2])
        MSE = float(SSE)/(thumb_size*thumb_size)
        if MSE <= MSE_cutoff:
            res['color'] = "grayscale"
        else:
            res['color'] = "color"
        res['mse'] = MSE
        res['mse int'] = int(MSE)
    elif len(bands)==1:
        res['color'] = 'black and white'
        res['bands'] = bands
    else:
        res['color'] = 'unknown'
        res['bands'] = bands
    return res


@click.command()
@click.argument('tag', nargs=-1)
@click.option('--access-key')
def main(tag, access_key):
    tags = tag
    client = hydrus.Client(access_key=access_key)
    fids = client.search_files(tags)
    mds = client.file_metadata(file_ids=fids, only_identifiers=True)

    for md in tqdm(mds):
        img_hash = md['hash']
        with NamedTemporaryFile(delete=False) as f:
            file_content = client.get_file(hash_=img_hash)
            with open(f.name, 'wb') as ff:
                ff.write(file_content)
                image_filename = f.name
                try:
                    pil_img = Image.open(image_filename)
                except OSError:
                    logging.exception('hash:{}'.format(img_hash))
                    continue
                fr_count = len(fr.face_locations(fr.load_image_file(image_filename)))
                try:
                    af_count = len(af.detect(pil_img))
                except ValueError:
                    logging.exception('hash:{}'.format(img_hash))
                    af_count = None  # type: ignore
                dc_res = detect_color_image(pil_img=pil_img)
                tags = []
                tags.append('face:fr {}'.format(fr_count))
                if af_count is not None:
                    tags.append('face:af {}'.format(af_count))
                if dc_res.get('mse', None):
                    tags.append('color_detect:mse {}'.format(dc_res['mse int']))
                if dc_res.get('color', '') in ('black and white', 'unknown'):
                    tags.append('color_detect:{} {}'.format(dc_res['color'], dc_res['bands']))

                tqdm.write('hash:{}\n{}'.format(img_hash, '\n'.join(tags)))
                client.add_tags([img_hash], services_to_tags={'local tags': tags})


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
