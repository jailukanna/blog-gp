---
title: tkinter example windowbutton (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'windowbutton'


Modules used in program: 
* `import tkinter.messagebox #needed for messagebox `
* `import tkinter `

## python windowbutton

Python tkinter example: windowbutton

```python
#Ian McTavish 
#July 26, 2016 
#Create a window with an Entry widget 
 
#import required libraries 
import tkinter 
import tkinter.messagebox #needed for messagebox 
 
class MyGUI: 
    """Graphics class"""
    #constructor method
    def __init__(self):
       """Initializes the window"""
       #Create the main window widget 
       self.main_window = tkinter.Tk() 

       #Create two frames 
       self.top_frame = tkinter.Frame(self.main_window) 
       self.bottom_frame = tkinter.Frame(self.main_window) 

       #Top Frame widgets 
       self.label_prompt = tkinter.Label(self.top_frame,
                                         text='Enter a distance in kilometers')
       self.entry_kilo = tkinter.Entry(self.top_frame,
                                       width=10)

       #Pack top frame widgets 
       self.label_prompt.pack(side="left") 
       self.entry_kilo.pack(side="left") 

       #Create the bottom widgets 
       #Create a button Widget 
       #Text of button: Convert 
       #calls button_convert_click method 
       self.button_convert = tkinter.Button(self.bottom_frame,
                                            text = "Convert",
                                            command=self.button_convert_click)
       #Create quit button - runs destroy method of the root widget 
       self.quit_button = tkinter.Button(self.bottom_frame,
                                         text='Quit',
                                         command=self.main_window.destroy)

       #Pack the button 
       self.button_convert.pack(side="left") 
       self.quit_button.pack(side="left") 

       #Pack the frames 
       self.top_frame.pack() 
       self.bottom_frame.pack() 

       #enter the mainloop 
       tkinter.mainloop() 

   #convert button click is callback function for the Convert button 
    def button_convert_click(self):
       #Get the value entered in the entry widget 
       kilo = float(self.entry_kilo.get()) 
       #convert km to miles 
       miles = kilo*0.6214 

       #Display an info dialog box 
       tkinter.messagebox.showinfo("Results",
                                   str(kilo) + " kilometers is "+
                                   str(miles) + " miles.")

#Create an instance of the MyGUI class
print(MyGUI.__doc__)
my_gui = MyGUI()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
