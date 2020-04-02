---
title: PIL example add image to django oscar product command (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'add image to django oscar product command'


Modules used in program: 
* `import os`
* `import io`

## python add image to django oscar product command

Python PIL example: add image to django oscar product command

```python
# -*- coding: utf-8 -*-
from django.core.management.base import BaseCommand, CommandError
from oscar.core.loading import get_model
from django.core.files import File
from PIL import Image
import io
from urllib.request import urlopen
from urllib.request import urlretrieve
import os
from oscar.apps.catalogue.exceptions import (
    IdenticalImageError, ImageImportError, InvalidImageArchive)

Product = get_model('catalogue', 'Product')
ProductImage = get_model('catalogue', 'ProductImage')


class Command(BaseCommand):
    help = 'Command to add an image gathered from a web source to a existing django oscar Product model'

    def add_arguments(self, parser):
        pass

    def handle(self, *args, **options):

        test_product = Product.objects.get(title='product-title')
        response_for_image_source = urlopen("http://image.source/")
        downloaded_image_as_tmp_file = urlretrieve("http://image.source/")

        image_file_for_pil_test = io.BytesIO(response_for_image_source.read())
        image_to_validate = Image.open(image_file_for_pil_test)
        # this call raises an exception when it tries to open an invalid image
        image_to_validate.verify()
        next_index = 0
        # check if the downloaded image already is attached to the product and if so raises an
        # exception to stop the saving process
        for existing_image in test_product.images.all():
            next_index = existing_image.display_order + 1
            try:
                download_image_in_byte_form = File(open(downloaded_image_as_tmp_file[0], 'rb')).read()

                if download_image_in_byte_form == existing_image.original.read():
                    raise IdenticalImageError()
            except IOError:
                # File probably doesn't exist
                existing_image.delete()
        im = ProductImage(product=test_product, display_order=next_index)
        image_name = os.path.basename("http://image.source/")
        im.original.save(
            image_name,
            File(open(downloaded_image_as_tmp_file[0], 'rb')), save=False
        )

        im.save()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
