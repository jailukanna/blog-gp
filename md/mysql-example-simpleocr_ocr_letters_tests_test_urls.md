---
title: mysql example simpleocr ocr letters tests test urls (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simpleocr ocr letters tests test urls'


## python simpleocr ocr letters tests test urls

Python mysql example: simpleocr ocr letters tests test urls

```python
from django.test import SimpleTestCase
from django.urls import reverse, resolve
from ocr_letters.views import add


class TestUrls(SimpleTestCase):

    # 测试url
    def test_add_url_is_resolved(self):
        url = reverse('ocr_letters:add')
        self.assertEquals(resolve(url).func, add)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
