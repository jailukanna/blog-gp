---
title: mysql example simpleocr ocr letters models (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simpleocr ocr letters models'


## python simpleocr ocr letters models

Python mysql example: simpleocr ocr letters models

```python
from django.db import models

# Create your models here.
from django.db import models
from django.utils import timezone


class mysqlImage(models.Model):
    mid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=20)
    letters = models.CharField(max_length=100)
    isdelete = models.IntegerField(default=0)
    addtime = models.DateTimeField(auto_now_add=True)
    modifytime = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = 'tb_ocr_letters'  # 数据表名称


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
