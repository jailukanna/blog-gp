---
title: mysql example simpleocr ocr letters tests test forms (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simpleocr ocr letters tests test forms'


## python simpleocr ocr letters tests test forms

Python mysql example: simpleocr ocr letters tests test forms

```python
from django.core.files.uploadedfile import SimpleUploadedFile
from django.test import SimpleTestCase
from ocr_letters.forms import AddForm


class TestForms(SimpleTestCase):

    def test_AddForm_valid_data_png(self):
        with open('./testimg/testpng.png', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('testpng.png', contents, content_type='image/png')
        form_data = {'name': 'lee'}
        form = AddForm(form_data, files={'headimg': test_img})
        self.assertTrue(form.is_valid())

    def test_AddForm_valid_data_jpg(self):
        with open('./testimg/timg.jpeg', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('timg.jpeg', contents, content_type='image/jpeg')
        form_data = {'name': 'chris'}
        form = AddForm(form_data, files={'headimg': test_img})
        self.assertTrue(form.is_valid())

    def test_AddForm_invalid_data_no_name(self):
        with open('./testimg/timg.jpeg', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('timg.jpeg', contents, content_type='image/jpeg')
        form_data = {'name': ''}
        form = AddForm(form_data, files={'headimg': test_img})
        self.assertFalse(form.is_valid())

    def test_AddForm_invalid_data_no_image(self):
        form_data = {'name': 'joe'}
        form = AddForm(form_data, files={'headimg': b''})
        self.assertFalse(form.is_valid())

    def test_AddForm_invalid_data_no_name_and_no_image(self):
        form_data = {'name': ''}
        form = AddForm(form_data, files={'headimg': b''})
        self.assertFalse(form.is_valid())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
