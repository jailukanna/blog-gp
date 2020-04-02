---
title: pil example django-thumbnail (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'django-thumbnail'


## python django-thumbnail

Python pil example: django-thumbnail

```python
# add the following util.py to your application directory

# in the models.py
from .util import create_thumbnail

class Article(models.Model):
    image = models.ImageField(default="")
    thumbnail = models.ImageField()
    def save(self):
        # create a thumbnail
        size = (200,200)
        create_thumbnail(self.image, self.thumbnail, size)
        super(Article, self).save()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
