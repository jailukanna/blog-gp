---
title: tkinter example tkinter1-2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkinter1-2'


Modules used in program: 
* `import tkinter.font as font`
* `import tkinter`

## python tkinter1-2

Python tkinter example: tkinter1-2

```python
import tkinter
import tkinter.font as font

root = tkinter.Tk()
root.title("Hello Tkinter World.")
root.geometry("600x500+100+100")

#ここから追加
labelFont = font.Font(size=30,weight="bold")#テキストのフォントを定義
label = tkinter.Label(root,text="This is test window!!!",font=labelFont)#表示するテキストの定義
label.pack(anchor="center",expand=1)#テキストの配置
#追加ここまで

root.mainloop()#画面表示

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
