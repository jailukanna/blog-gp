---
title: PIL example hydrus jupyter face annotation (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'hydrus jupyter face annotation'


Modules used in program: 
* `import hydrus`
* `import face_recognition`
* `import os`
* `import io`

## python hydrus jupyter face annotation

Python PIL example: hydrus jupyter face annotation

```python
"""
require face_recognition, tqdm, pillow, pigeon-jupyter, jupyter
access_key = 'HYDRUS_ACCESS_KEY'
fa = FaceAnnotation(access_key)
annotations = fa.annotate('tag request', ['option1', 'option2])
"""
import io
import os

import face_recognition
from IPython.display import Image, display
from pigeon import annotate
from PIL import Image as PilImage
from PIL import ImageDraw
from tqdm import tqdm_notebook

import hydrus
from hydrus.utils import yield_chunks


class FaceAnnotation:
    def __init__(self, access_key):
        self.image_cache = {}
        self.cl = hydrus.Client(access_key)

    def get_image(self, hash_value):
        if hash_value not in self.image_cache:
            self.image_cache[hash_value] = self.cl.get_file(hash_value)
        return self.image_cache[hash_value]

    def get_annotation_data(self, tag, max_hash_count=None):
        # get hashes from tag
        f_ids = self.cl.search_files([tag])
        if not f_ids:
            return
        if max_hash_count and len(f_ids) > max_hash_count:
            f_ids = f_ids[:max_hash_count]
        f_ids_chunks = hydrus.utils.yield_chunks(f_ids, 256)
        metadata = []
        for chunk in f_ids_chunks:
            metadata.extend(self.cl.file_metadata(file_ids=chunk, only_identifiers=True))
        hashes = [x['hash'] for x in metadata]
        for hash_value in tqdm_notebook(hashes):
            face_locations = []
            # todo: search face location using face_recognition
            image = face_recognition.load_image_file(io.BytesIO(self.get_image(hash_value)))
            face_locations = face_recognition.face_locations(image)
            if face_locations:
                for face_location in face_locations:
                    yield [hash_value, face_location]

    def display_fn(self, image_info, max_height=None):
        hash_value, face_location = image_info
        raw_img = self.get_image(hash_value)
        img = PilImage.open(io.BytesIO(raw_img))
        draw = ImageDraw.Draw(img)
        fl = face_location  # top, right, bottom, left
        args = [fl[3], fl[0], fl[1], fl[2]]  # [x0, y0, x1, y1]
        draw.rectangle(args, outline='red')
        #  __import__('pdb').set_trace()
        img_bytes = io.BytesIO()
        if max_height is not None:
            width, height = img.size
            new_height = max_height
            new_width  = new_height * width / height
            img.thumbnail((new_width, new_height), PilImage.ANTIALIAS)
        img.save(img_bytes, format='JPEG')
        display(Image(img_bytes.getvalue()))

    def annotate(self,tag, options, max_height=None, max_hash_count=None):
        return annotate(
            self.get_annotation_data(tag, max_hash_count),
            options=options,
            display_fn=lambda x: self.display_fn(x, max_height)
        )


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
