---
title: mysql example simpleocr ocr letters tests test views (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simpleocr ocr letters tests test views'


## python simpleocr ocr letters tests test views

Python mysql example: simpleocr ocr letters tests test views

```python
from django.core.files.uploadedfile import SimpleUploadedFile
from django.test import TestCase, Client
from django.urls import reverse


class TestViews(TestCase):
    def setUp(self):
        self.client = Client()
        self.add_url = reverse('ocr_letters:add')

    # 测试png图片可以上传并且返回json结果
    def test_add_POST_png(self):
        with open('./testimg/testpng.png', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('testpng.png', contents, content_type='image/png')
        response = self.client.post(self.add_url, data={'name': 'frank', 'headimg': test_img})
        self.assertEquals(response.status_code, 200)
        self.assertJSONEqual(response.content,
                             {"content": ["F", "O", "L", "L", "O", "W", "D", "R", "E", "A", "M", "S", ",",
                                          "t", "h", "e", "t", "a", "K", "N", "O", "W", "V", "A", "T"]})

    # 测试jpg图片可以上传并且返回json结果
    def test_add_POST_jpg(self):
        with open('./testimg/timg.jpeg', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('timg.jpeg', contents, content_type='image/jpeg')
        response = self.client.post(self.add_url, data={'name': 'frank', 'headimg': test_img})
        self.assertEquals(response.status_code, 200)
        self.assertJSONEqual(response.content,
                             {"content": ["\u00bb", "C", "A", "S", "A", "D", "O", "S", "V", "I", "N", "H", "O", "S"]})

    # 测试txt文档不是图片不能上传，应当返回当前页面
    def test_add_POST_txt_invalid(self):
        with open('./testimg/testtxtfile.txt', 'rb') as file:
            contents = file.read()
        test_img = SimpleUploadedFile('testtxtfile.txt', contents, content_type='text/plain')
        response = self.client.post(self.add_url, data={'name': 'frank', 'headimg': test_img})
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'users/add.html')

    # 测试txt文档不是图片不能上传，并且没有用户名输入，应当返回当前页面
    def test_add_POST_txt_invalid_no_name(self):
        with open('./testimg/testtxtfile.txt', 'rb') as file:
            contents = file.read()
        test_img = SimpleUploadedFile('testtxtfile.txt', contents, content_type='text/plain')
        response = self.client.post(self.add_url, data={'name': '', 'headimg': test_img})
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'users/add.html')

    # 测试用户不上传用户名的情况，应当返回当前页面
    def test_add_POST_no_name_submit(self):
        with open('./testimg/testpng.png', 'rb') as img:
            contents = img.read()
        test_img = SimpleUploadedFile('testpng.png', contents, content_type='image/png')
        response = self.client.post(self.add_url, data={'name': '', 'headimg': test_img})
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'users/add.html')

    # 测试用户不上传图片的情况，应当返回当前页面
    def test_add_POST_no_image_submit(self):
        response = self.client.post(self.add_url, data={'name': 'eric', 'headimg': b''})
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'users/add.html')

    # 测试用户既不上传图片也不上传用户名的情况，应当返回当前页面
    def test_add_POST_no_image_and_no_name_submit(self):
        response = self.client.post(self.add_url, data={'name': '', 'headimg': b''})
        self.assertEquals(response.status_code, 200)
        self.assertTemplateUsed(response, 'users/add.html')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
