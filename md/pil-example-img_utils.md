---
title: pil example img utils (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'img utils'

Functions in program: 
* `def change_xml_path(path):`
* `def change_img_format(path):`
* `def filename_to_txt(target_path,output_path):`
* `def resize_img(resize_width=480,resize_height=736,input_path ="./input_example/*",output_path = "./output_example/"):`
* `def add_margin(img,resize_width,resize_height):`
* `def scale_box(img, width, height):`
* `def change_file_name(path):`

Modules used in program: 
* `import xml.etree.ElementTree as ET`
* `import os`
* `import glob`
* `import numpy as np`
* `import cv2`
* `import glob`

## python img utils

Python pil example: img utils

```python
"""
必要なモジュール
"""
import glob
import cv2
import numpy as np
import glob
import os
from scipy.ndimage.interpolation import rotate
from scipy.misc import imresize
from PIL import Image
import xml.etree.ElementTree as ET


"""
画像のファイル名を変更する関数
"""
def change_file_name(path):
    """
    Parameters
    ファイルのパス　例:path="./test/*.jpg"
    """
    target_list = glob.glob(path)
    current_dir = os.getcwd()
    for i in range(len(target_list)):
        file_type = os.path.basename(target_list[i]).split(".")[1]
        if len(str(i)) == 1:
            filename = "img0"+str(i)+"."+file_type
        else:
            filename = "img"+str(i)+"."+file_type

        save_path = os.path.join(current_dir,filename)
        os.rename(target_list[i],save_path)
        print("{}>>{}".format(target_list[i],save_path))

"""
画像のサイズを変更する関数
"""
# アスペクト比固定で指定サイズ内に収まるようリサイズ
def scale_box(img, width, height):
    scale = min(width / img.shape[1], height / img.shape[0])
    return cv2.resize(img, dsize=None, fx=scale, fy=scale)

# 余白部分を黒で埋める
def add_margin(img,resize_width,resize_height):
    height = img.shape[0]
    width = img.shape[1]
    diff_height = resize_height - height
    diff_width = resize_width - width

    # 画像の縦幅を変更
    if diff_height <0:
        pass
    elif diff_height == 0:
        pass
    elif diff_height >0:
        while img.shape[0] <resize_height:
            img = np.insert(img, img.shape[0], 0, axis=0)

    # 画像の横幅を変更
    if diff_width <0:
        pass
    elif diff_width == 0:
        pass
    elif diff_width >0:
        while img.shape[1] <resize_width:
            img = np.insert(img, img.shape[1], 0, axis=1)
    return img

def resize_img(resize_width=480,resize_height=736,input_path ="./input_example/*",output_path = "./output_example/"):
    path_list = glob.glob(input_path)

    if os.path.exists(output_path):
        pass
    else:
        os.mkdir(output_path)

    for i in range(len(path_list)):
        filename = os.path.basename(path_list[i])
        print(filename)
        img = cv2.imread(path_list[i])
        scaled_img = scale_box(img,resize_width,resize_height)
        resize_img = add_margin(scaled_img,resize_width,resize_height)
        cv2.imwrite(os.path.join(output_path,filename),resize_img)
        print(f"{img.shape} -> {resize_img.shape}")
        
"""
画像のファイル名をテキストファイルにする関数
"""
def filename_to_txt(target_path,output_path):
    """
    Parameters
    target_path:画像ファイルのパス 例:path="./test/*.jpg"
    output_path:テキストファイル形式のパス 例 "./hoge.txt"
    """
    path_list = glob.glob(target_path)
    img_name_list = []

    for i in range(len(path_list)):
        img_name_list.append(os.path.basename(path_list[i]).split(".")[0])

    with open(output_path, mode="w") as f:
        f.write("\n".join(img_name_list))

"""
画像のフォーマットを変更する関数
"""
def change_img_format(path):
    """
    Parameters
    ファイルのパス　例:path="./test/*.jpg"
    """

    save_dir = "./format changed Images"
    path_list = glob.glob(path)

    if os.path.exists(save_dir):
        pass
    else:
        os.mkdir(save_dir)

    for i in range(len(path_list)):
        filename = os.path.basename(path_list[i]).split(".")[0]+".jpg"
        save_path = os.path.join(save_dir,filename)
        pil_img = Image.open(path_list[i],"r")
        pil_img = pil_img.convert("RGB")
        pil_img.save(save_path,"JPEG")

"""
アノテーションファイル(xml)のpathを.png→.jpgに変更する関数
"""
def change_xml_path(path):

    """
    <filename>と<path>を変更
    """
    path_list = glob.glob(path)
    for i in range(len(path_list)):
        tree = ET.parse(path_list[i])
        root = tree.getroot()
        filename = root.find("filename").text.split(".")[0] + ".jpg"
        pathname = root.find("path").text.split(".")[0] + ".jpg"
        root.find("filename").text = filename
        root.find("path").text = pathname
        tree.write(path_list[i])

"""
画像枚数を増やすクラス
"""
class augmentor():
    def __init__(self,path,flip_rate,rotate_rate,scale_rate):
        self.img_list = glob.glob(path)
        self.flip_rate = flip_rate
        self.rotate_rate = rotate_rate
        self.scale_rate = scale_rate

    """
    水平フリップ
    """
    def holizon_flip(self):
        holizon_flip_img_list = []
        for i in range(len(self.img_list)):
            if np.random.rand() <self.flip_rate:
                img = cv2.imread(self.img_list[i])
                img = img[:, ::-1, :] #水平Flip
                holizon_flip_img_list.append(img)
                self.save_img(img,os.path.basename(self.img_list[i]),"holizon_flip")
        return holizon_flip_img_list

    """
    垂直フリップ
    """
    def vertical_flip(self):
        vertical_flip_img_list = []
        for i in range(len(self.img_list)):
            if np.random.rand() <self.flip_rate:
                img = cv2.imread(self.img_list[i])
                img = img[::-1, :, :] #垂直Flip
                vertical_flip_img_list.append(img)
                self.save_img(img,os.path.basename(self.img_list[i]),"vertical_flip")
        return vertical_flip_img_list

    """
    回転
    """
    def random_rotate(self,angle_range=(-10,10)):
        rotate_img_list=[]
        for i in range(len(self.img_list)):
            if np.random.rand() <self.rotate_rate:
                img = cv2.imread(self.img_list[i])
                h, w, _ = img.shape
                angle = np.random.randint(*angle_range)
                if angle == 0:
                    pass
                else:
                    img = rotate(img, angle)
                    img = imresize(img, (h, w))
                    rotate_img_list.append(img)
                    self.save_img(img,os.path.basename(self.img_list[i]),"rotate")
        return rotate_img_list

    """
    拡大
    """
    def scale_up(self):
        scale_up_img_list = []
        for i in range(len(self.img_list)):
            if np.random.rand() < self.rotate_rate:
                img = cv2.imread(self.img_list[i])
                scale_rate = np.random.randint(70,90)*0.01
                h,w, _ = img.shape
                h_two_edge_size = h - h*scale_rate
                w_two_edge_size = w - w*scale_rate
                h_edge_size = int(h_two_edge_size/2)
                w_edge_size = int(w_two_edge_size/2)
                img_scale_up = img[h_edge_size:h-h_edge_size,w_edge_size:w-w_edge_size,:]
                img_scale_up_resize = imresize(img_scale_up, (h, w))
                self.save_img(img_scale_up_resize,os.path.basename(self.img_list[i]),"scale_up")
                scale_up_img_list.append(img_scale_up)
        return scale_up_img_list

    """
    縮小
    """
    def scale_down(self):
        scale_down_img_list = []
        for i in range(len(self.img_list)):
            if np.random.rand() < self.rotate_rate:
                img =cv2.imread(self.img_list[i])
                h,w, _ = img.shape
                scale_rate = np.random.randint(60,80)*0.01
                img_scale_down = cv2.resize(img,(int(w*scale_rate),int(h*scale_rate)))

                h_down,w_down,_ = img_scale_down.shape

                diff_height = h - h_down
                diff_width = w - w_down

                # 画像の縦幅を変更
                for j in range(diff_height):
                    if j < diff_height/2:
                        img_scale_down = np.insert(img_scale_down,0,0,axis=0)
                    else:
                        img_scale_down = np.insert(img_scale_down,img_scale_down.shape[0],0,axis=0)

                # 画像の横幅を変更
                for j in range(diff_width):
                    if j < diff_width/2:
                        img_scale_down = np.insert(img_scale_down,0,0,axis=1)
                    else:
                        img_scale_down = np.insert(img_scale_down,img_scale_down.shape[1],0,axis=1)
                self.save_img(img_scale_down,os.path.basename(self.img_list[i]),"scale_down")
                scale_down_img_list.append(img_scale_down)
        return scale_down_img_list

    """
    保存
    """
    def save_img(self,target_img,img_name,effect,save_dir="./aug"):
        """
        parameters
        target_img:numpy配列として格納された画像
        img_name:画像のファイル名
        effect:画像に加えた処理
        save_dir:保存先のフォルダ名
        """
        if os.path.exists(save_dir):
            pass
        else:
            os.mkdir("./aug")
        save_path = os.path.join(save_dir,img_name.split(".")[0]+"_"+effect+"."+img_name.split(".")[1])
        cv2.imwrite(save_path,target_img)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
