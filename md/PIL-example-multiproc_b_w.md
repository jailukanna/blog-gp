---
title: PIL example multiproc b w (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'multiproc b w'

Functions in program: 
* `def process_job(char, value, verified_font_paths, test_ratio, test_images_dir,train_images_dir,width, height, need_crop, margin):`
* `def args_parse():`
* `def get_label_dict():`

Modules used in program: 
* `import multiprocessing`
* `import sys`
* `import copy`
* `import traceback`
* `import shutil`
* `import numpy as np`
* `import random`
* `import json`
* `import os`
* `import fnmatch`
* `import time`
* `import argparse`
* `import pickle`

## python multiproc b w

Python PIL example: multiproc b w

```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-

from __future__ import print_function

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
from PIL import ImageFilter,ImageStat
import pickle
import argparse
from argparse import RawTextHelpFormatter
from multiprocessing import Pool
import time
import fnmatch
import os
import json
import random
import numpy as np
import shutil
import traceback
import copy
import sys

from twisted.internet.threads import deferToThreadPool
from twisted.internet import reactor
from twisted.internet.defer import Deferred
import multiprocessing


class FontCheck(object):

    def __init__(self, lang_chars, width=32, height=32):
        self.lang_chars = lang_chars
        self.width = width
        self.height = height

    def do(self, font_path):
        width = self.width
        height = self.height
        try:
            for i, char in enumerate(self.lang_chars):
                img = Image.new("RGB", (width, height), "black")  # 黑色背景
                draw = ImageDraw.Draw(img)
                font = ImageFont.truetype(font_path, int(width * 0.9), )
                # 白色字体
                draw.text((0, 0), char, (255, 255, 255),
                          font=font)
                data = list(img.getdata())
                sum_val = 0
                for i_data in data:
                    sum_val += sum(i_data)
                if sum_val < 2:
                    return False
        except:
            print("fail to load:%s" % font_path)
            traceback.print_exc(file=sys.stdout)
            return False
        return True


# 生成字体图像
class Font2Image(object):

    def __init__(self,
                 width, height,
                 need_crop, margin):
        self.width = width
        self.height = height
        self.need_crop = need_crop
        self.margin = margin


    def do(self, font_path, char, rotate=0):
        try:
            font = ImageFont.truetype(font_path, int(self.height * 0.3))    #字体/图像大小比例
            width, height = font.getsize(char)
            img = Image.new('RGB', (self.width, self.height), (255, 255, 255))
            im_width, im_height = img.size
            pos = ((im_width - width) / 2, (im_height - height) / 2)

            draw = ImageDraw.Draw(img)
            draw.text(pos, char, fill='gray', font=font)

            rotate = random.choice([-45,-40,-35,-30,-25,-20,-15,-10,-5,0,5,10,15,20,25,30,35,40,45])
            img = img.rotate(rotate)

            data = list(img.getdata())
            np_img = np.asarray(data, dtype='uint8')
            np_img = np_img[:, 0]      #单通道
            np_img = np_img.reshape((self.height, self.width))
            np_img = np_img[9:49, 9:49]
            img = Image.fromarray(np_img).convert('L')
            return img
        except:
            traceback.print_exc()
            raise


# 注意，chinese_labels里面的映射关系是：（ID：汉字）
def get_label_dict():
    f = open('./chinese_labels', 'rb')
    label_dict = pickle.load(f)
    f.close()
    return label_dict



def args_parse():
    # 解析输入参数
    parser = argparse.ArgumentParser(
        description=description, formatter_class=RawTextHelpFormatter)
    parser.add_argument('--out_dir', dest='out_dir',
                        default=None, required=True,
                        help='write a caffe dir')
    parser.add_argument('--font_dir', dest='font_dir',
                        default=None, required=True,
                        help='font dir to to produce images')
    parser.add_argument('--test_ratio', dest='test_ratio',
                        default=0.2, required=False,
                        help='test dataset size')
    parser.add_argument('--width', dest='width',
                        default=None, required=True,
                        help='width')
    parser.add_argument('--height', dest='height',
                        default=None, required=True,
                        help='height')
    parser.add_argument('--no_crop', dest='no_crop',
                        default=True, required=False,
                        help='', action='store_true')
    parser.add_argument('--margin', dest='margin',
                        default=0, required=False,
                        help='', )
    parser.add_argument('--rotate', dest='rotate',
                        default=0, required=False,
                        help='max rotate degree 0-45')
    parser.add_argument('--rotate_step', dest='rotate_step',
                        default=0, required=False,
                        help='rotate step for the rotate angle')
    parser.add_argument('--need_aug', dest='need_aug',
                        default=False, required=False,
                        help='need data augmentation', action='store_true')
    args = vars(parser.parse_args())
    return args

def process_job(char, value, verified_font_paths, test_ratio, test_images_dir,train_images_dir,width, height, need_crop, margin):
    print(char, value)
    font2image = Font2Image(width, height, need_crop, margin)
    image_list = []



    for j, verified_font_path in enumerate(verified_font_paths):  # 内层循环是字体
        image = font2image.do(verified_font_path, char)
        image_list.append(image)
    test_num = len(image_list) * test_ratio
    random.shuffle(image_list)  # 图像列表打乱
    count = 0
    for i in range(len(image_list)):
        img = image_list[i]
        # print(img.shape)
        if count < test_num:
            char_dir = os.path.join(test_images_dir, "%0.5d" % value)
        else:
            char_dir = os.path.join(train_images_dir, "%0.5d" % value)

        if not os.path.isdir(char_dir):
            os.makedirs(char_dir)
        path_image = os.path.join(char_dir, "%d.png" % count)
        img.save(path_image)
        count += 1

if __name__ == "__main__":
    start = time.time()
    description = '''
python gen_printed_char2.py --out_dir ./dataset \
			--font_dir ./chinese_fonts \
			--width 30 --height 30 --margin 4 --rotate 30 --rotate_step 1
    '''
    options = args_parse()
    out_dir = os.path.expanduser(options['out_dir'])
    font_dir = os.path.expanduser(options['font_dir'])
    test_ratio = float(options['test_ratio'])
    width = int(options['width'])
    height = int(options['height'])
    need_crop = not options['no_crop']
    margin = int(options['margin'])
    rotate = int(options['rotate'])
    need_aug = options['need_aug']
    rotate_step = int(options['rotate_step'])
    train_image_dir_name = "train"
    test_image_dir_name = "test"

    # 将dataset分为train和test两个文件夹分别存储
    train_images_dir = os.path.join(out_dir, train_image_dir_name)
    test_images_dir = os.path.join(out_dir, test_image_dir_name)

    if os.path.isdir(train_images_dir):
        shutil.rmtree(train_images_dir)
    os.makedirs(train_images_dir)

    if os.path.isdir(test_images_dir):
        shutil.rmtree(test_images_dir)
    os.makedirs(test_images_dir)

    # 将汉字的label读入，得到（ID：汉字）的映射表label_dict
    label_dict = get_label_dict()

    char_list = []  # 汉字列表
    value_list = []  # label列表
    for (value, chars) in label_dict.items():
        print(value, chars)
        char_list.append(chars)
        value_list.append(value)

    # 合并成新的映射关系表：（汉字：ID）
    lang_chars = dict(zip(char_list, value_list))
    font_check = FontCheck(lang_chars)

    if rotate < 0:
        roate = - rotate

    if rotate > 0 and rotate <= 45:
        all_rotate_angles = []
        for i in range(0, rotate + 1, rotate_step):
            all_rotate_angles.append(i)
        for i in range(-rotate, 0, rotate_step):
            all_rotate_angles.append(i)
        # print(all_rotate_angles)

    # 对于每类字体进行小批量测试
    verified_font_paths = []
    ## search for file fonts
    for font_name in os.listdir(font_dir):
        path_font_file = os.path.join(font_dir, font_name)
        if font_check.do(path_font_file):
            verified_font_paths.append(path_font_file)

    # font2image = Font2Image(width, height, need_crop, margin)

    ####################################### 进程池
    k = 0
    pool = multiprocessing.Pool()
    for (char, value) in lang_chars.items():
        k += 1
        pool.apply_async(process_job, (char, value,verified_font_paths, test_ratio, test_images_dir,
                                         train_images_dir, width, height, need_crop, margin))
    pool.close()
    pool.join()

    print("主进程结束，总耗时%s" % (time.time() - start), k)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
