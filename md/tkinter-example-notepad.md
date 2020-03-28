---
title: tkinter example notepad (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'notepad'

Functions in program: 
* `def hakkinda():`
* `def yapistir():`
* `def kopyala():`
* `def kes():`
* `def kaydet():`
* `def ac():`
* `def yeni():`

Modules used in program: 
* `import os`
* `import tkinter.filedialog`
* `import tkinter.messagebox`
* `import tkinter`

## python notepad

Python tkinter example: notepad

```python
import tkinter
import tkinter.messagebox
import tkinter.filedialog
import os

dosya = None

def yeni():
    global dosya
    dosya = None
    textBox.delete(1.0,"end")
def ac():
    global dosya
    dosya = tkinter.filedialog.askopenfilename(defaultextension=".txt",filetypes=[("Tüm Dosyalar","*.*"),("Text Dosyaları","*.txt")])
    textBox.delete(1.0,"end")
    oku = open(dosya,"r")
    textBox.insert(1.0,dosya.read())
    oku.close()
def kaydet():
    global dosya
    if(dosya == None):
        dosya = tkinter.filedialog.asksaveasfilename(initialfile='Untitled.txt',defaultextension=".txt",filetypes=[("Tüm Dosyalar","*.*"),("Text Dosyaları","*.txt")])
    yaz = open(dosya,"w")
    yaz.write(textBox.get(1.0,"end"))
    yaz.close()
def kes():
    textBox.event_generate("<<Cut>>")
def kopyala():
    textBox.event_generate("<<Copy>>")
def yapistir():
    textBox.event_generate("<<Paste>>")
def hakkinda():
    tkinter.messagebox.showinfo("Hakkında","Ben yaptım")

frmMain = tkinter.Tk()
textBox = tkinter.Text(frmMain)
scrollBar = tkinter.Scrollbar(textBox)
textBox.grid(sticky = tkinter.N + tkinter.E + tkinter.S + tkinter.W)

frmMain.geometry('500x300')

frmMain.grid_rowconfigure(0, weight=1)
frmMain.grid_columnconfigure(0, weight=1)

menuBar = tkinter.Menu(frmMain)
dosyaMenusu = tkinter.Menu(menuBar)
duzenMenusu = tkinter.Menu(menuBar)
yardimMenusu = tkinter.Menu(menuBar)

dosyaMenusu.add_command(label = "Yeni", command = yeni)
dosyaMenusu.add_command(label = "Aç", command = ac)
dosyaMenusu.add_command(label = "Kaydet", command = kaydet)

duzenMenusu.add_command(label = "Kes", command = kes)
duzenMenusu.add_command(label = "Kopyala", command = kopyala)
duzenMenusu.add_command(label = "Yapıştır", command = yapistir)

yardimMenusu.add_command(label = "Hakkında",command = hakkinda)

menuBar.add_cascade(label = "Dosya",menu = dosyaMenusu)
menuBar.add_cascade(label = "Düzen",menu = duzenMenusu)
menuBar.add_cascade(label = "Yardım",menu = yardimMenusu)

frmMain.config(menu = menuBar)
scrollBar.pack(side=tkinter.RIGHT,fill=tkinter.Y) 

frmMain.mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
