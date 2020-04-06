---
title: mysql example simpleocr ocr letters views (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simpleocr ocr letters views'

Functions in program: 
* `def add(request):`

Modules used in program: 
* `import pytesseract`
* `import os`
* `import datetime`

## python simpleocr ocr letters views

Python mysql example: simpleocr ocr letters views

```python
import datetime
import os
from django.shortcuts import render
from ocr_letters.forms import AddForm
from ocr_letters.models import mysqlImage
from django.http import JsonResponse
import pytesseract
from PIL import Image

from simpleocr import settings

"""
 django.http模块中定义了HttpResponse 对象的API
 作用：不需要调用模板直接返回数据
 HttpResponse属性：
    content: 返回内容,字符串类型
    charset: 响应的编码字符集
    status_code: HTTP响应的状态码
"""


def add(request):
    # 判断是否为 post 方法提交
    if request.method == "POST":
        af = AddForm(request.POST, request.FILES)
        # 判断表单值是否和法
        if af.is_valid():
            name = af.cleaned_data['name']
            headimg = af.cleaned_data['headimg']
            img_type = headimg.content_type
            if img_type == "image/png" or img_type == "image/jpeg":
                # 把用户上传的图片存储到本地 media 文件夹中
                fix = datetime.datetime.now().strftime('%Y%m%d%H%M%S%f')
                img_path = os.path.join(settings.MEDIA_ROOT, fix + headimg.name)
                f = open(img_path, 'wb')
                for i in headimg.chunks():
                    f.write(i)
                f.close()
                # 调用pytesseract处理图片
                image = Image.open(headimg)
                content = pytesseract.image_to_string(image)
                # 识别内容后进行字符串处理，移除空行，空格
                content1 = "".join([s for s in content.splitlines(True) if s.strip()])
                content2 = ''.join(content1.split(' '))
                content3 = "".join(content2.splitlines())  # content3 是最后处理完的字符串
                # 创建要放到Json里返回出去的字典并把识别结果放进一个数组里
                char_dict = {'content': ''}
                arr = []
                for char in content3:
                    arr.append(char)
                # 把数组作为value存在content键里
                char_dict['content'] = arr
                # 封装用户上传信息到mysqlImage对象
                mysqlimage = mysqlImage(name=name, letters=content3, isdelete=0)
                mysqlimage.save()
                return JsonResponse(char_dict, safe=False)
            else:
                return render(request, 'users/add.html', context={"af": af})
        else:
            return render(request, 'users/add.html', context={"af": af})
    else:
        # 不合法继续返回add.html页面
        af = AddForm()
        return render(request, 'users/add.html', context={"af": af})


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
