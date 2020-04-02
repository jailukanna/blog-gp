---
title: PIL example IPAI (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'IPAI'

Functions in program: 
* `def gui():`
* `def photofunc(serial_queue):`
* `def aifunc():`
* `def serialreadfunc(serial_queue):`

Modules used in program: 
* `import tkinter as tk`
* `import numpy as np`
* `import cv2`
* `import numpy as np`
* `import time`
* `import serial`

## python IPAI

Python PIL example: IPAI

```python
import serial
import time
from multiprocessing import Process,Queue
import numpy as np
import cv2
from PIL import Image,ImageTk
from keras.models import load_model
import numpy as np
import tkinter as tk


serial_queue    = Queue()
i = 30
cap = cv2.VideoCapture(0)

def serialreadfunc(serial_queue):
    print("in_read")
    ser = serial.Serial('COM5',9600)
    ser.flushInput()
    l = 0
    while all:
        ser.write(b'*')
        time.sleep(0.1)
        c = ser.inWaiting()
        if(c != 0):
            break
        l += 1

    time.sleep(0.1)
    c = ser.inWaiting()      #(bites)
    ser.read(c)


    #test
    print(l)
    serialnumber = 0

    while all:
        ser.flush()
        time.sleep(0.5)
        c = ser.inWaiting()
        serialnumber = ser.read(c)
        serialnumber = int.from_bytes(serialnumber, 'big')
        if(serialnumber == 1):
            serial_queue.put(serialnumber)
        elif(serialnumber == 2):
            serial_queue.put(serialnumber)
        else:
            serial_queue.put(serialnumber)

def aifunc():
    print("ai")
    image = Image.open("photo.jpg")
    image = image.resize((64,64))

    model = load_model("highschool_model.h5")
    np_image = np.array(image)
    np_image = np_image / 255
    np_image = np_image[np.newaxis, :, :, :]
    result = model.predict(np_image)

    return(result)


def photofunc(serial_queue):
    print("in_photo")


    delay = 1
    while(True):
        serialnumber = serial_queue.get()
        ret,frame = cap.read()
        cv2.imshow('frame',frame)
        if(serialnumber == 2):
            pass


        if(serialnumber == 1):
            print("shut")
            path = "photo.jpg"
            cv2.imwrite(path,frame)        
            p_guifunc=Process(target = gui,args=())
            p_guifunc.start()

        if(cv2.waitKey(delay) & 0xFF == ord('q')):
            pass

def gui():
    print("gui")
    result = aifunc() 
    if result[0][0] > result[0][1]:
        print("boy",result[0][0])
        result  = float(result[0][0]) *100
        print(result)
        result  = str(result)
        print(result)
        result  = result[0:6] + "%"
        result_str = "boy : "  +   result

    else:
        print("girl",result[0][1])
        result = float(result[0][1])*100
        print(result)
        result  = str(result)
        print(result)
        result  = result[0:6] + "%"
        result_str = "girl : "  +   result
    root = tk.Tk()
    root.geometry("640x480")
    root.title("ImageProcessing_AI")
    resultlabel = tk.Label(root,font=("System",50,'normal'))
    resultlabel.pack(fill='x')
    resultlabel.config(text=result_str)
    exit_time = tk.Label(root,font=(20))
    exit_time.pack()

    image_bgr = cv2.imread("photo.jpg")
    image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB) # imreadはBGRなのでRGBに変換
    image_pil = Image.fromarray(image_rgb) # RGBからPILフォーマットへ変換
    image_tk  = ImageTk.PhotoImage(image_pil) # ImageTkフォーマットへ変換

    canvas = tk.Canvas(root, width=image_bgr.shape[0], height=image_bgr.shape[1]) # Canvas作成
    canvas.pack()
    canvas.create_image(0, 0, image=image_tk, anchor='nw') # ImageTk 画像配置

    def exit_gui():
        global i

        i -= 1
        i_s = str(i)
        exit_time.config(text="表示終了まで後"+i_s+"秒")
        if(i==0):
            exit()
        
        root.after(1000, exit_gui)
    exit_gui()
    root.mainloop()


    

p_serialreadfunc   = Process(target = serialreadfunc,args=(serial_queue,))
p_photofunc        = Process(target = photofunc     ,args=(serial_queue,))


if(__name__ == '__main__'):
    p_serialreadfunc.start()
    p_photofunc.start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
