---
title: PIL example gui-1 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gui-1'


Modules used in program: 
* `import cv2`
* `import PIL.Image`
* `import Tkinter as tk`
* `import cv2`
* `import Tkinter `

## python gui-1

Python PIL example: gui-1

```python
# -*- coding: utf-8 -*-
#%%
# import Image and the graphics package Tkinter
import Tkinter 
from PIL import Image, ImageTk

# open a SPIDER image and convert to byte
#format
im = Image.open('pic.jpg')

root = Tkinter.Tk()  
# A root window for displaying objects

 # Convert the Image object into a TkPhoto 
#object
tkimage = ImageTk.PhotoImage(im)

Tkinter.Label(root, image=tkimage).pack()
# Put it in the display window

root.mainloop() # Start the GUI
#%%
import cv2
import Tkinter as tk
window = tk.Toplevel()  #Makes main window
window.wm_title("Face Recognition")
window.config(background="#111111")
#window.geometry("900x500")
#Graphics window
imageFrame = tk.Frame(window, width=900, height=500)
imageFrame.grid(row=0, column=0, padx=10, pady=2)
#Button frames
buttonFrame = tk.Frame(window, width=200, height=500, padx=600)


#Capture video frames
lmain = tk.Label(imageFrame)
lmain.grid(row=0, column=0)
#cap = cv2.VideoCapture(0)
frame=cv2.imread('pic.jpg')
cv2image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGBA)
img = Image.fromarray(cv2image)
imgtk = ImageTk.PhotoImage(image=img)
lmain.imgtk = imgtk
lmain.configure(image=imgtk)

window.mainloop()

#%%
from PIL import ImageTk
import PIL.Image
from Tkinter import *
import cv2

class Application(Frame):
    """a gui application with  buttons"""
    
    def __init__(self,master):
        """initialize the frame"""
        Frame.__init__(self,master)
        self.grid()
        self.Button_clicks=0
        self.create_widgets()
        
            
   
    def create_widgets(self):
        """create  3 buttons"""
        #create the label
        self.instruction=Label(self,text="enter the password")
        self.instruction.grid(row=0,column=0,columnspan=2,sticky=W)
        
        self.password=Entry(self)
        self.password.grid(row=1,column=1,sticky=W)
        
        self.submit=Button(self,text="SUBMIT",command=self.reveal)
        self.submit.grid(row=2,column=0,sticky=W)
        
        self.message=Text(width=35,height=5,wrap=WORD)
        self.message.grid(row=3,column=0,columnspan=2,sticky=W)
        
        img=Image.Open('pic.jpg')

        photo = ImageTk.PhotoImage(img)
        self.canvas=Label(self,image=photo)
        canvas.image=photo
        self.canvas.grid(row=4,column=0)
        
        

    def reveal(self):
        """increase the click count and display the total """
        content=self.password.get()
        
        if content=="password":
            msg="welcome"
            
        else:
            msg="get lost"
        self.message.delete(0.0,END)    
        self.message.insert(0.0,msg)


root=Tk()
app=Application(root)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
