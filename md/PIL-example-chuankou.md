---
title: PIL example chuankou (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'chuankou'


Modules used in program: 
* `import string       #串口所用的库`
* `import serial.tools.list_ports`
* `import serial`
* `import win32ui   #图像取模所用的库`
* `import re`
* `import numpy as np`
* `import os`

## python chuankou

Python PIL example: chuankou

```python
# coding=UTF-8
import os
from PIL import Image
import numpy as np
from numpy import array
import re
import win32ui   #图像取模所用的库
import serial
import serial.tools.list_ports
import string       #串口所用的库
dlg = win32ui.CreateFileDialog(1) # 1表示打开文件对话框
dlg.SetOFNInitialDir('E:/Python') # 设置打开文件对话框中的初始显示目录
dlg.DoModal()
filename = dlg.GetPathName() # 获取选择的文件名称
print(filename)
pil_im = Image.open(filename).convert('1')  #转为黑白图
(weight,high) = pil_im.size
if (high - weight) > 0:
    pil_im = pil_im.rotate(270)
    (weight,high) = pil_im.size
if(weight > 128):
    weight_new = 128
    high_new = high*weight_new/weight
    pil_im = pil_im.resize((high_new,weight_new),Image.ANTIALIAS)
if(high > 64):
    high_new = 64
    weight_new = weight*high_new/high
    pil_im = pil_im.resize((high_new,weight_new),Image.ANTIALIAS)
#heibai = pil_im.convert("1")
#heibai.show()
#pil_im.save("out.bmp")
juzhen = array(pil_im, np.uint8)
h,w=juzhen.shape
#print(juzhen)
if w < 128:
    wcha= 128 - w
    for Hangbian in range(0,wcha,1):
        tmp=np.ones((h), np.uint8)
        #print(tmp)
        juzhen = np.insert(juzhen, w+Hangbian, values=tmp, axis=1)
    w = 128
if h < 64:
    hcha= 64 - h
    for Liebian in range(0,hcha,1):
        tmp = np.ones((w), np.uint8)
        juzhen = np.insert(juzhen,h+Liebian,values=tmp,axis=0)
    h = 64
if w > 128:
    wcha= 128 - w
    juzhen = np.delete(juzhen, wcha, axis=1)
if h > 64:
    hcha = 64 - h
    juzhen = np.delete(juzhen,hcha,axis=0)
#print(juzhen.shape, juzhen.dtype ,w , h)
#for i in range(w):
#    for j in range(h):
#        if juzhen[j, i] == 0:
#            juzhen[j, i] = 0
#        else:
#            juzhen[j, i] = 1
#dlg = win32ui.CreateFileDialog(1) # 1表示打开文件对话框
#dlg.SetOFNInitialDir('E:/Python') # 设置打开文件对话框中的初始显示目录
#dlg.DoModal()
#outname = dlg.GetPathName() # 获取选择的文件名称
#print(outname)
#f = open(outname + '.txt', 'w')
cmd=[]
a = [0 for i in range(0, 8)]
for y in range(0,56,8):
    for x in range(0,128,1):
        for t in range(0,8):
            if juzhen[y+t,x]:
                a[t]=0
            else:
                a[t]=1
        INT = a[0] * 1 + a[1] * 2 + a[2] * 4 + a[3] * 8 + a[4] * 16 + a[5] * 32 + a[6] * 64 + a[7] * 128
        HEX =hex(INT)
        cmd.append(INT)

plist=list(serial.tools.list_ports.comports())
if len(plist)<=0:
    print(("please input devices"))
    #exit(0)
else:
    print(("find the ports as follow\n"))
    print((plist[0]))
portname=input("please input port number\n")
bote=raw_input("please input bo te lv \n")
bote_num=int(bote)
ser=serial.Serial("COM"+str(portname),bote_num)
#ser=serial.Serial("COM"+str(portname),1200)
lalala=[0x0f,0xee]
lencha=len(cmd)%10
for i in range(0,len(cmd)-lencha-10,10):
    cmd_temp=cmd[i:(i+10)]
    ser.write(cmd_temp)
print(("succesful\n"))



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
